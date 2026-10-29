---
id: spring-ai-document-reader-office-pdf-email
url: parser/java/use-cases/spring-ai-document-reader-office-pdf-email
title: "Spring AI DocumentReader for Office, PDF and Email: Integration Guide"
weight: 3
description: "Plug GroupDocs.Parser into the Spring AI ETL pipeline as a DocumentReader: page-level Documents from Word and PDF, email attachments and ZIP members with lineage metadata."
keywords: Spring AI DocumentReader, Spring AI ETL pipeline, Tika alternative for Spring AI, read DOCX and PDF in Spring AI, GroupDocs.Parser for Java, RAG with Spring AI, TokenTextSplitter, parse email attachments in Java, Spring Boot integration, Java 17
productName: GroupDocs.Parser for Java
structuredData:
    showOrganization: True
toc: true
hideChildren: False
draft: true
---

{{< alert style="info" >}}
💡 Full working example available on GitHub:
[spring-ai-document-reader-using-groupdocs-parser-java](https://github.com/groupdocs-parser/spring-ai-document-reader-using-groupdocs-parser-java)
{{< /alert >}}

## Overview

**A Spring AI DocumentReader for Office, PDF and email** is a GroupDocs.Parser capability for Java that lets a Spring AI application read Word, PDF, EML/MSG and ZIP resources into `Document` objects through the standard `DocumentReader` interface. Spring AI defines ingestion as an ETL pipeline: a reader supplies Documents, a transformer such as `TokenTextSplitter` cuts them into chunks, and a `VectorStore` stores the embeddings. This guide integrates one class from the sample project, `GroupDocsParserDocumentReader`, into that pipeline. You need it when your corpus contains mailbox exports, archives and long manuals, and your RAG answers must cite a page and a parent file. The reader runs in-process, detects formats from content, and emits one Document per page plus one set per attachment. By the end of the page you will have the reader running against your own files and know which constructor and settings fit each input.

**What this guide covers:**
- Adding GroupDocs.Parser and `spring-ai-commons` to a Maven project
- Three integration patterns: single resource, containers, batch ETL
- Constructor options and the metadata keys each Document carries
- Error handling and a checklist for production

{{< alert style="warning" >}}
⚠️ Java 17 is the minimum. Spring AI 2.x, and therefore `spring-ai-commons` 2.0.2, needs it.
{{< /alert >}}

## Quickstart

Get the reader working in your project in minutes:

**Step 1 - Install the package**

Declare the GroupDocs repository `https://releases.groupdocs.com/java/repo/` in `pom.xml`, then add both dependencies:

```xml
<dependency>
    <groupId>com.groupdocs</groupId>
    <artifactId>groupdocs-parser</artifactId>
    <version>26.10</version>
</dependency>
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-commons</artifactId>
    <version>2.0.2</version>
</dependency>
```

**Step 2 - Add to your project**

Copy `GroupDocsParserDocumentReader.java` from the repository and read files with it, as `readAll` in the demo does:

```java
List<Document> documents = new ArrayList<>();
for (File file : files) {
    GroupDocsParserDocumentReader reader =
            new GroupDocsParserDocumentReader(new FileSystemResource(file));
    documents.addAll(reader.read());
}
return documents;
```

**Step 3 - Run it**

In the sample project, `mvn compile exec:java` reads the 4 files in `resources/`, prints the chunk count per source and ends with `Documents read: 12, chunks: 20`. The chunks are written to `output/spring-ai-documents.jsonl`, one JSON object per line.

## Prerequisites

- JDK 17 or later and Apache Maven
- GroupDocs.Parser for Java 26.10 and `org.springframework.ai:spring-ai-commons` 2.0.2
- A license file for production; without it the library runs in evaluation mode

## Core Concepts

Before diving into integration patterns, understand these key concepts:

- **DocumentReader** - Spring AI's reader contract extends `Supplier<List<Document>>`; `read()` is a default method that calls `get()`. Any class that implements `get()` can replace another reader.
- **Metadata keys** - the reader sets `source`, `file_type`, `page_count`, `page_number` (1-based, page mode only) and `parent_source` (attachments only). `TokenTextSplitter` adds `chunk_index`, `total_chunks` and `parent_document_id`.
- **Container items** - GroupDocs.Parser exposes email attachments and ZIP entries as `ContainerItem` objects; each one opens its own `Parser`.

## Integration Patterns

### Pattern 1: Drop-In Reader for a Single Resource

Use this pattern wherever your code already builds a reader per `Resource`, for example in an upload handler. `get()` opens the stream, creates a `Parser` and delegates to `readBody`, so the format is detected from the bytes rather than the file name.

```java
try (InputStream stream = resource.getInputStream();
     Parser parser = new Parser(stream)) {
    List<Document> documents = new ArrayList<>(readBody(parser, source, null));
    if (includeAttachments) {
        documents.addAll(readAttachments(parser, source));
    }
    return documents;
}
```

`readBody` then chooses between one Document per page and one for the whole file:

```java
if (pagePerDocument && pageCount > 0 && parser.getFeatures().isTextPage()) {
    for (int page = 0; page < pageCount; page++) {
        String text = readText(parser.getText(page));
        Map<String, Object> metadata =
                metadata(source, parent, fileType, pageCount);
        metadata.put(METADATA_PAGE, page + 1);
        addIfNotBlank(documents, text, metadata);
    }
} else {
    String text = readText(parser.getText());
    addIfNotBlank(documents, text, metadata(source, parent, fileType, pageCount));
}
```

**Key points:**
- `fileType` and `pageCount` come from `getDocumentInfo()` before any text is read.
- GroupDocs.Parser page indexes are 0-based; `page_number` is stored as `page + 1`.
- Blank pages never become Documents, so the embedding model receives no empty strings.

### Pattern 2: Mailboxes and Archives with Lineage

Use the single-argument constructor for `.eml`, `.msg` and `.zip` inputs. It turns on `includeAttachments`, and `readAttachments` feeds every container item back through `readBody`.

```java
Iterable<ContainerItem> items = parser.getContainer();
if (items == null) {
    return documents;
}
for (ContainerItem item : items) {
    String childSource = source + "/" + item.getFilePath();
    try (Parser child = item.openParser()) {
        documents.addAll(readBody(child, childSource, source));
    } catch (UnsupportedDocumentFormatException ex) {
        System.out.println("skipped (unsupported format): " + childSource);
    }
}
```

**Key points:**
- A chunk from the archive carries `source` = `sample.zip/sample.pdf` and `parent_source` = `sample.zip`.
- `getContainer()` returns `null` for non-container formats, so the same code is safe for plain PDFs.
- In the bundled EML, `attachment.txt` produces a Document; an image item returns no `TextReader` without OCR, so it adds nothing.

### Pattern 3: Batch ETL into a Vector Store

Use this pattern in a scheduled job. The demo reads every file with Pattern 1, then applies Spring AI's splitter:

```java
TokenTextSplitter splitter = TokenTextSplitter.builder()
        .withChunkSize(chunkSizeTokens)
        .build();
return splitter.apply(documents);
```

The demo's `writeJsonl` writes the chunks to a file where your code would call `vectorStore.add(chunks)`.

**Key points:**
- The splitter copies the reader's metadata to every chunk, so filters on `parent_source` work at query time.
- With a chunk size of 300 tokens, the 12 sample Documents become 20 chunks.
- In Spring Boot, take the resources from a `ResourceLoader` or an upload instead of `File`.

## Configuration Reference

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `pagePerDocument` | boolean | `true` | One Document per page with `page_number`; `false` returns one Document per file. |
| `includeAttachments` | boolean | `true` | Reads email attachments and archive members as separate Documents. |
| `chunkSizeTokens` | int | `300` in the demo | Chunk size passed to `TokenTextSplitter.builder().withChunkSize`. |
| `licensePath` | String | `REPLACE_WITH_YOUR_LICENSE_PATH` | Path to the `.lic` file used in `applyLicense()`. |

The two-flag constructor is `GroupDocsParserDocumentReader(resource, pagePerDocument, includeAttachments)`.

## Error Handling

The reader never lets one bad file stop a batch. An unsupported format is logged and returns an empty list; an I/O error becomes an `UncheckedIOException`, because `get()` cannot declare checked exceptions.

**Common errors and how to fix them:**

| Error | Cause | Fix |
|-------|-------|-----|
| `skipped (unsupported format)` in the log | GroupDocs.Parser cannot read the file or item | Check the [supported formats](https://docs.groupdocs.com/parser/java/supported-document-formats/); route the line to your logger |
| `UncheckedIOException` | The `Resource` stream could not be opened or read | Check the path, permissions or upload size |
| `UnsupportedClassVersionError` | Spring AI 2.x classes on a JVM older than 17 | Run on JDK 17+ and keep `maven.compiler.release` at 17 |

## When to Use This Approach

The deciding factor is the shape of the corpus. A PDF-only archive is well served by `PagePdfDocumentReader`, which the [Spring AI ETL pipeline reference](https://docs.spring.io/spring-ai/reference/api/etl-pipeline.html) describes as built on Apache PDFBox. Standalone Office and HTML files may be fine with `TikaDocumentReader`, built on Apache Tika. Mailbox exports and ZIP bundles are where Pattern 2 earns its place, because the content that matters sits one level down. Test before you commit: point two readers at the same 50 files and compare the chunks retrieval returns for ten real questions.

## Common Pitfalls

- **Paging off for long files.** `pagePerDocument=false` drops `page_number`; keep it on for manuals and contracts.
- **Expecting text from images.** The JPEG members of `sample.zip` produce nothing without OCR.
- **Ignoring the skip log.** It is the only signal that a file never reached the index.
- **Null metadata values.** Spring AI rejects them; if you add your own keys, follow the reader's pattern of adding a key only when it has a value.

## Production Readiness Checklist

Before deploying to production, verify the following:

- [ ] JDK 17+ on every node that runs ingestion
- [ ] License path set in `applyLicense()` and loaded once at startup
- [ ] `page_number` and `parent_source` visible in a sample of stored chunks
- [ ] Chunk size matched to your embedding model
- [ ] `writeJsonl` replaced with your `VectorStore`
- [ ] Error handling is implemented for all edge cases
- [ ] Logging is in place for the reader's skip messages

## Frequently Asked Questions

### How do I swap TikaDocumentReader for GroupDocs.Parser in a Spring AI ETL pipeline?

Replace the reader constructor and keep everything after it. Where the pipeline builds `new TikaDocumentReader(resource)`, build `new GroupDocsParserDocumentReader(resource)` instead; both implement `DocumentReader`, so `read()` returns the same `List<Document>` type. `TokenTextSplitter` and the `VectorStore` are called exactly as before, and the Documents now carry `page_number`, `page_count`, `file_type` and, for attachments, `parent_source`.

**Q: Does the reader need a file extension?**
A: No. The `Parser` is created from `resource.getInputStream()` and detects the format from content. When `getFilename()` returns null, `source` falls back to `"resource"`.

**Q: Can I get one Document per file instead of per page?**
A: Yes. Use the three-argument constructor with `pagePerDocument` set to `false`; `page_count` is still recorded. I still keep page mode as my default, because the first question a reviewer asks about a generated answer is which page it came from.

**Q: Does any document leave the server?**
A: No. GroupDocs.Parser runs inside the JVM, and the sample makes no network calls while parsing.

## Conclusion

A GroupDocs.Parser-backed `DocumentReader` adds page numbers and attachment lineage to a Spring AI pipeline with a one-line change at the reader stage. Run the sample, read `output/spring-ai-documents.jsonl`, then point the reader at a copy of your own mailbox export or document share before wiring your `VectorStore`.

## See Also

- [Blog: stop losing email attachments in a Spring AI RAG index](https://blog.groupdocs.com/parser/spring-ai-document-reader-office-pdf-email-java/)
- [Extract attachments from emails](https://docs.groupdocs.com/parser/java/extract-attachments-from-emails/) - the container API behind Pattern 2
- [Extract text from documents](https://docs.groupdocs.com/parser/java/extract-text-from-documents/) - `getText` and `TextReader` basics
- [Product documentation](https://docs.groupdocs.com/parser/java/) - getting started and advanced topics
- [API Reference](https://reference.groupdocs.com/parser/java/) - full API details for GroupDocs.Parser for Java
- [GitHub repository](https://github.com/groupdocs-parser/spring-ai-document-reader-using-groupdocs-parser-java) - complete source code
