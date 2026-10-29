---
id: extract-annotations-from-microsoft-office-word-documents
url: parser/java/extract-annotations-from-microsoft-office-word-documents
title: Extract Annotations from Microsoft Office Word documents
weight: 2
description: "This article explains that how to extract annotations from Microsoft Office Word (.doc, .docx) documents"
keywords: extract annotations, extract annotations from Microsoft Office Word, .doc, .docx
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---

Microsoft Office Word documents (.doc, .docx) often contain **annotations** – comments left by reviewers. With **GroupDocs.Parser for Java**, you can easily read these annotations programmatically using the [getAnnotations](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getAnnotations()) method.

This guide shows how to extract annotations from Microsoft Office Word documents in Java step by step.

---

## What Are Word Annotations?

Annotations are comments that reviewers attach to a Word (.doc, .docx) document during a review pass. The `getAnnotations` method reads these comments and returns them as a collection of [AnnotationItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/AnnotationItem) objects, each exposing a `getValue()` method with the comment's text, along with metadata such as the author and timestamp.

The method has two overloads:

| Overload | Description |
| --- | --- |
| [getAnnotations()](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getAnnotations()) | Extracts annotations from the whole Word document |
| [getAnnotations(int pageIndex)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getAnnotations(int)) | Extracts annotations from a specific page (zero-based index) |

⚡ *Note: Annotation extraction is supported for word processing documents: DOC, DOT, DOCX, DOCM, DOTX, DOTM, ODT, OTT and RTF. Plain text (TXT) files don't support annotations, so [getAnnotations](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getAnnotations()) returns *null* for them.*

---

## How to Extract Annotations from a Word Document in Java

Follow these steps to get annotations from a .doc or .docx file:

1. **Create a `Parser` object** and load the Word document.
2. **Call the `getAnnotations` method** to retrieve the annotation collection.
3. **Iterate through the collection** and read each annotation's value.

### Example: Extract Word Annotations in Java

```java
// Create an instance of Parser class
try (Parser parser = new Parser("MyDocument.docx")) {
    // Extract annotations
    Iterable<AnnotationItem> annotations = parser.getAnnotations();

    // Check if annotation extraction is supported
    if (annotations == null) {
        System.out.println("Annotation extraction isn't supported.");
        return;
    }

    // Display annotations
    for (AnnotationItem item : annotations) {
        System.out.println(item.getValue());
    }
}
```

{{< alert style="warning" >}}
[getAnnotations](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getAnnotations()) method returns *null* if annotation extraction isn't supported for the document. If the Word document has no comments, [getAnnotations](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getAnnotations()) method returns an empty collection.
{{< /alert >}}

## Extract Word Text Together with Annotations

Sometimes it's useful to get the document's text and its comments in one pass, instead of calling `getAnnotations` separately. Call `setIncludeAnnotations(true)` on [TextOptions](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/TextOptions) and pass it to the [getText(TextOptions)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getText(com.groupdocs.parser.options.TextOptions)) method – the comment text will be included in the extracted output alongside the regular document text.

### Example: Extract Text with Annotations in Java

```java
// Create an instance of Parser class
try (Parser parser = new Parser("MyDocument.docx")) {
    // Create the options to extract text in accurate mode and include annotations
    TextOptions options = new TextOptions(false);
    options.setIncludeAnnotations(true);

    // Extract a text into the reader
    try (TextReader reader = parser.getText(options)) {
        // Print a text with annotations
        System.out.println(reader == null ? "Text extraction isn't supported." : reader.readToEnd());
    }
}
```

## Reading Word Annotation Metadata

Each [AnnotationItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/AnnotationItem) also exposes metadata that helps trace who left a comment in a document and when:

| Method | Type | Description |
| --- | --- | --- |
| `getAnnotationType()` | `String` | The annotation type (`Comment` for Word comments). |
| `getTimestamp()` | `java.util.Date` | Date and time when the comment was created or last modified; `null` if the document doesn't provide it. |
| `getAuthorName()` | `String` | Name of the author who created or last modified the comment. |
| `getAuthorInitials()` | `String` | Initials of the author who created or last modified the comment. |

### Example: Reading Annotation Metadata in Java

```java
// Create an instance of Parser class
try (Parser parser = new Parser("MyDocument.docx")) {
    // Check if the document supports annotation extraction
    if (!parser.getFeatures().isAnnotations()) {
        System.out.println("Document isn't supports annotation extraction.");
        return;
    }
    // Get the document info
    IDocumentInfo documentInfo = parser.getDocumentInfo();
    // Iterate over pages
    for (int p = 0; p < documentInfo.getPageCount(); p++) {
        // Extract annotations from the page
        for (AnnotationItem annotation : parser.getAnnotations(p)) {
            System.out.println(String.format("Page: %d, Author: %s (%s)",
                    p, annotation.getAuthorName(), annotation.getAuthorInitials()));
            System.out.println(String.format("Created/Modified: %s, Type: %s",
                    annotation.getTimestamp(), annotation.getAnnotationType()));
            System.out.println("Comment: " + annotation.getValue());
        }
    }
}
```

## Why Extract Annotations from Word Documents?

Extracting comments from Microsoft Office Word documents is useful for:

*   Review workflows – collect reviewer feedback left in documents during a review cycle.
*   Collaboration – surface document comments without opening Word.
*   Auditing – track who commented on a document, and when, over time.

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
