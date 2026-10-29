---
id: prepare-documents-rag-markdown-chunks
url: parser/java/use-cases/prepare-documents-rag-markdown-chunks
title: "Prepare Documents for RAG in Java: A Six-Stage Markdown Chunking Architecture"
weight: 30
description: "Architecture of an on-premise RAG ingestion pipeline in Java: Word, PDF, email and ZIP files become heading-scoped Markdown chunks with source and parent lineage in JSON Lines."
keywords: document parsing for RAG, Java RAG ingestion, convert documents to Markdown in Java, chunk documents by headings, GroupDocs.Parser for Java, extract email attachments in Java, on-premise document parser, LLM-ready Markdown
productName: GroupDocs.Parser for Java
structuredData:
    showOrganization: True
toc: True
hideChildren: False
draft: false
---

{{< alert style="info" >}}
💡 Full working example available on GitHub:
[document-to-markdown-rag-chunks-java](https://github.com/groupdocs-parser/document-to-markdown-rag-chunks-java)
{{< /alert >}}

## Executive Summary

Heading-scoped RAG chunking is a GroupDocs.Parser capability for Java that converts business documents to Markdown, splits them at headings, and keeps a record of which file, attachment or archive member each piece came from. The architecture on this page is a single-process pipeline of six stages, each one a static method in `RagChunksDemo.java`. It accepts DOCX, PDF, EML and ZIP inputs, writes one JSON object per chunk, and never sends a file outside the JVM that runs it. With the four bundled inputs it produces 22 chunks. That output is framework-agnostic: any embedding job or vector store that reads JSON Lines can load it.

The 2026 comparisons of RAG parsers mostly weigh Python projects against hosted, metered services. A Java team bound by data-residency rules can use neither as-is, and that gap is what this design addresses.

## What This Guide Covers

The page groups the six methods into three architectural layers. I find that grouping easier to reason about than six separate steps, because each layer has one job and one failure mode. For each layer it shows the real code, the design choice behind it and what to watch in production. Read it if you own a Java RAG ingestion service and need to decide where parsing, chunking and lineage tracking live.

**Prerequisites:**
- JDK 11 or later for the shipped `pom.xml` (the source compiles on Java 8 if you lower the compiler level) and Maven 3.x.
- GroupDocs.Parser for Java 26.10; a [temporary licence](https://purchase.groupdocs.com/temp-license/100326) removes evaluation limits.

## Installation

Add the GroupDocs repository and the single dependency to `pom.xml`:

```xml
<repositories>
    <repository>
        <id>groupdocs-artifact-repository</id>
        <url>https://releases.groupdocs.com/java/repo/</url>
    </repository>
</repositories>
<dependencies>
    <dependency>
        <groupId>com.groupdocs</groupId>
        <artifactId>groupdocs-parser</artifactId>
        <version>26.10</version>
    </dependency>
</dependencies>
```

Then run `mvn compile exec:java` from the project root, where the demo expects its `resources/` folder.

## Repository Structure

```
document-to-markdown-rag-chunks-java/
├── README.md
├── pom.xml
├── resources/
│   ├── SampleWithToc.docx
│   ├── email-with-attachment.eml
│   ├── sample.pdf
│   └── sample.zip
└── src/main/java/com/groupdocs/demo/RagChunksDemo.java
```

## Stage Matrix

| Stage | Method | GroupDocs.Parser API | Failure handling | Output |
|-------|--------|----------------------|------------------|--------|
| Open | `chunkDocument` | `new Parser(path)` | Unsupported format skipped | Chunk list per file |
| Route | `chunkParser` | none (orchestration) | Depth limit of 2 | Body chunks, then child chunks |
| Extract | `extractMarkdown` | `getFormattedText`, `getText` | Falls back, then `none` | Text plus format label |
| Chunk | `chunkByHeadings` | none (pure Java) | Blank sections dropped | Chunks with heading paths |
| Expand | `chunkContainerItems` | `getContainer`, `openParser` | Unreadable items skipped | Child chunks with parent id |
| Emit | `writeJsonl` | none (JDK I/O) | I/O errors propagate | `output/chunks.jsonl` |

## Layer 1: Ingestion and Recursion

### Design Overview

The entry point opens one file and hands an open `Parser` to a recursive router. Keeping the router parser-based rather than path-based is the important decision: an attachment pulled out of an email is also an open `Parser`, so top-level files and nested items share the exact same path through the code. The recursion depth is capped by `MAX_CONTAINER_DEPTH`, set to 2.

**Design Pattern:** recursive descent over a document tree  
**Concurrency Model:** synchronous, one file per call  
**State Management:** stateless; lineage travels as method arguments

### Implementation Architecture

`chunkDocument` sets the source id to the file name and the parent to `null`:

```java
String sourceId = new File(path).getName();
try (Parser parser = new Parser(path)) {
    return chunkParser(parser, sourceId, null, 0);
} catch (UnsupportedDocumentFormatException ex) {
    System.out.println("skipped (unsupported format): " + sourceId);
    return new ArrayList<>();
}
```

`chunkParser` then extracts, chunks and descends:

```java
ExtractedText extracted = extractMarkdown(parser);
List<Chunk> chunks =
        chunkByHeadings(extracted.text, sourceId, parentId, extracted.format);
if (depth < MAX_CONTAINER_DEPTH) {
    chunks.addAll(chunkContainerItems(parser, sourceId, depth));
}
return chunks;
```

### Scalability Analysis

**Horizontal Scaling:** each call to `chunkDocument` is independent, so files can be spread across worker threads or queue consumers without shared state.  
**Vertical Scaling:** memory follows the size of the largest extracted text, because the reader output is held as one string.  
**Bottlenecks:** large archives are processed sequentially inside one call; split them upstream if a single ZIP holds hundreds of members.

## Layer 2: Extraction and Heading Chunking

### Design Overview

Extraction asks the library what the format supports instead of hard-coding a list. If `getFeatures().isFormattedText()` is true, the text comes back as Markdown with headings, lists and tables intact; otherwise `getText()` supplies plain text. In the sample run the DOCX and the email body come back as Markdown, while the PDF takes the plain-text path. The chunker then walks the Markdown line by line and keeps a six-level heading stack.

**Design Pattern:** capability probe with graceful fallback, followed by a stack-based splitter  
**Concurrency Model:** synchronous, CPU-bound string work  
**State Management:** the heading stack lives only for one document

### Implementation Architecture

The extraction stage returns text and a label that says which mode produced it:

```java
if (parser.getFeatures().isFormattedText()) {
    FormattedTextOptions options =
            new FormattedTextOptions(FormattedTextMode.Markdown);
    try (TextReader reader = parser.getFormattedText(options)) {
        if (reader != null) {
            return new ExtractedText("markdown", reader.readToEnd());
        }
    } catch (IOException ex) {
        throw new RuntimeException(ex);
    }
}
try (TextReader reader = parser.getText()) {
    if (reader != null) {
        return new ExtractedText("plain", reader.readToEnd());
    }
} catch (IOException ex) {
    throw new RuntimeException(ex);
}
return new ExtractedText("none", "");
```

The chunking stage closes a section at every heading and rebuilds the path:

```java
for (String line : markdown.split("\\r?\\n", -1)) {
    Matcher heading = HEADING.matcher(line);
    if (heading.matches()) {
        addSection(chunks, section.toString(), sourceId, parentId, path, format);
        section.setLength(0);
        int level = heading.group(1).length();
        stack[level - 1] = heading.group(2).trim();
        for (int i = level; i < stack.length; i++) {
            stack[i] = null;
        }
        path = joinPath(stack);
    }
    section.append(line).append('\n');
}
```

For `SampleWithToc.docx` this yields paths such as `Heading 1 > Heading 1.2 > Heading 1.2.1`. Sections longer than 1500 characters (`MAX_CHUNK_CHARS`) are cut at paragraph breaks by the `addSection` helper.

### Operational Considerations

- **Monitoring:** count chunks per `format` value; a jump in `plain` or a file with zero chunks usually means scanned input.
- **Error Handling:** `IOException` from a reader is rethrown, so a corrupt stream fails the file instead of producing silent gaps.

## Layer 3: Container Expansion and Output

### Design Overview

Emails and archives are treated as small document trees. `getContainer()` returns the attachments of an EML or MSG message or the entries of a ZIP, and `null` for anything else. Each item opens its own `Parser` and re-enters the router with the container's id as parent. The final stage writes everything as UTF-8 JSON Lines without a JSON library.

**Design Pattern:** composite traversal plus streaming serializer  
**Concurrency Model:** synchronous  
**State Management:** parent id passed down, chunk list returned up

### Implementation Architecture

```java
Iterable<ContainerItem> items = parser.getContainer();
if (items == null) {
    return chunks;
}
for (ContainerItem item : items) {
    String childId = sourceId + "/" + item.getFilePath();
    try (Parser child = item.openParser()) {
        chunks.addAll(chunkParser(child, childId, sourceId, depth + 1));
    } catch (UnsupportedDocumentFormatException ex) {
        System.out.println("skipped (unsupported format): " + childId);
    }
}
```

```java
for (Chunk c : chunks) {
    out.write("{\"id\":" + json(c.id)
            + ",\"source\":" + json(c.source)
            + ",\"parent\":" + json(c.parent)
            + ",\"heading_path\":" + json(c.headingPath)
            + ",\"format\":" + json(c.format)
            + ",\"text\":" + json(c.text) + "}\n");
}
```

## How do I keep an email attachment linked to its message in a RAG index?

Store the `parent` field as vector metadata. The pipeline builds every child id from the container id plus the item path, so the text file attached to the sample email becomes chunk `email-with-attachment.eml/attachment.txt#0` with `parent` set to `email-with-attachment.eml`. At query time, read `parent` from the matched chunk and show or fetch the original message next to the answer.

## When to Use This Approach

Choose this design when the files must stay on your servers and the ingestion service is written in Java. It suits mixed inboxes and document shares where Word files, PDFs, mail exports and ZIP uploads arrive together, because one code path handles all of them. It is also the right fit when answers need citations, since every chunk carries a file name and a heading path. If your corpus is mostly scanned paper, add an OCR step first; this pipeline reads only text the documents already contain.

## Common Pitfalls

- **Running from the wrong folder.** `DOCS` and `RESULT` are relative paths, so `mvn exec:java` must start in the project root or the demo exits with code 1.
- **Expecting headings from PDFs.** PDF text arrives as `plain` here, so its chunks have an empty `heading_path`; filter or weight them accordingly.
- **Dropping the `none` signal.** Images in `sample.zip` produce no chunks at all, which is easy to miss unless you log files with zero output.
- **Raising the depth limit casually.** A higher `MAX_CONTAINER_DEPTH` lets crafted archives multiply work; I would keep 2 unless a real mail export needs 3.

## Security and Compliance

Parsing runs in-process, so file content never crosses the network and indexing volume does not turn into a per-page invoice. That keeps the pipeline inside the same audit boundary as the document store. The licence call in `applyLicense()` reads a local file and leaves the documents untouched.

{{< alert style="warning" >}} ⚠️ The repository ships with `REPLACE_WITH_YOUR_LICENSE_PATH`. Never commit a real `.lic` file next to the code.{{< /alert >}}

## Notes

Chunk ids restart at `#0` for each document, so the pair of `id` and `source` is unique while the index alone is not. Output is written to `output/chunks.jsonl`, and the process exits with code 2 when no chunks were produced, which lets a CI job detect an empty run.

## FAQ

**Does this depend on a RAG or LLM framework?**
No. The only dependency is GroupDocs.Parser for Java, and the output is plain JSON Lines. You can load it into a vector store from Java, Python or a command-line tool. The field names are the only contract between the two sides.

**Can I process a document from a stream instead of a path?**
The demo opens files by path in `chunkDocument`, but `chunkParser` accepts any open `Parser`. Create the `Parser` from your stream, pass it in with a source id of your choosing, and the rest of the pipeline is unchanged. Container items already work this way through `openParser()`.

**What happens with formats GroupDocs.Parser cannot read?**
`UnsupportedDocumentFormatException` is caught at both the file and the container-item level. The file or item is reported on the console and skipped. Processing of the remaining inputs continues.

## Conclusion and Recommendations

The six stages fit in one class, yet they cover the three things a RAG index needs from ingestion: structured text, sensible chunk boundaries and traceable lineage. Start by running the demo against a folder of your own files and opening `output/chunks.jsonl` to check the heading paths. Then move `chunkDocument` behind your existing queue and map the six JSON fields to your vector store's metadata.

## See Also

- [On-premise RAG ingestion in Java: the blog article for this project](https://blog.groupdocs.com/parser/prepare-documents-rag-markdown-chunks-java/)
- [Extract Markdown formatted text with GroupDocs.Parser for Java](https://docs.groupdocs.com/parser/java/markdown/)
- [Extract attachments from emails in Java](https://docs.groupdocs.com/parser/java/extract-attachments-from-emails/)
- [GroupDocs.Parser for Java documentation](https://docs.groupdocs.com/parser/java/)
- [GroupDocs.Parser for Java API reference](https://reference.groupdocs.com/parser/java/)
- [GitHub repository](https://github.com/groupdocs-parser/document-to-markdown-rag-chunks-java): complete source code and sample files
