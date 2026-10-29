---
id: extract-annotations-from-pdf-documents
url: parser/java/extract-annotations-from-pdf-documents
title: Extract Annotations from PDF Documents in Java
weight: 2
description: "Learn how to extract annotations from PDF files in Java using GroupDocs.Parser for Java."
keywords: extract PDF annotations Java, PDF annotations Java, read PDF document annotations Java, get PDF file annotations, GroupDocs.Parser annotations extraction
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---

PDF files often contain **annotations** – comments, notes, highlights, and other markup added by reviewers. With **GroupDocs.Parser for Java**, you can easily read these annotations programmatically using the [getAnnotations](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getAnnotations()) method.

This guide shows how to extract annotations from PDF documents in Java step by step.

---

## What Are PDF Annotations?

Annotations are notes, comments, and markup that reviewers add on top of a PDF document's content – for example, sticky notes, text comments, or highlighted remarks. The `getAnnotations` method reads this markup and returns it as a collection of [AnnotationItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/AnnotationItem) objects, each exposing a `getValue()` method with the annotation's text. Annotations without text content are skipped.

The method has two overloads:

| Overload | Description |
| --- | --- |
| [getAnnotations()](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getAnnotations()) | Extracts annotations from the whole document |
| [getAnnotations(int pageIndex)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getAnnotations(int)) | Extracts annotations from a specific page (zero-based index) |

⚡ *Note: Annotation support depends on the document format. Use [getFeatures().isAnnotations()](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isAnnotations()) to check whether the loaded document supports annotation extraction.*

---

## How to Extract PDF Annotations in Java

Follow these steps to get annotations from a PDF document:

1. **Create a `Parser` object** and load the PDF file.
2. **Call the `getAnnotations` method** to retrieve the annotation collection.
3. **Iterate through the collection** and read each annotation's value.

### Example: Extract PDF Annotations in Java

```java
// Create an instance of Parser class
try (Parser parser = new Parser("MyDocument.pdf")) {
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
[getAnnotations](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getAnnotations()) method returns *null* if annotation extraction isn't supported for the document. If the PDF document has no annotations, [getAnnotations](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getAnnotations()) method returns an empty collection.
{{< /alert >}}

### Example: Extract Annotations from Each Page in Java

```java
// Create an instance of Parser class
try (Parser parser = new Parser("MyDocument.pdf")) {
    // Check if the document supports annotation extraction
    if (!parser.getFeatures().isAnnotations()) {
        System.out.println("Document isn't supports annotation extraction.");
        return;
    }
    // Get the document info
    IDocumentInfo documentInfo = parser.getDocumentInfo();
    // Iterate over pages
    for (int p = 0; p < documentInfo.getPageCount(); p++) {
        System.out.println(String.format("Page %d/%d", p + 1, documentInfo.getPageCount()));
        // Extract annotations from the page
        for (AnnotationItem item : parser.getAnnotations(p)) {
            System.out.println(item.getValue());
        }
    }
}
```

## Additional Annotation Metadata

The [AnnotationItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/AnnotationItem) class exposes extra metadata that gives more context about each annotation:

| Method | Type | Description |
| --- | --- | --- |
| `getAnnotationType()` | `String` | The annotation type (for PDF documents, the type of the PDF annotation). |
| `getTimestamp()` | `java.util.Date` | Date and time when the annotation was created or last modified; `null` if the document doesn't provide it. |
| `getAuthorName()` | `String` | Name of the author who created or last modified the annotation; `null` if the document doesn't provide it. |
| `getAuthorInitials()` | `String` | Initials of the author; `null` if the document doesn't provide it (PDF annotations don't store initials). |

### Example: Read Annotation Metadata in Java

```java
// Create an instance of Parser class
try (Parser parser = new Parser("MyDocument.pdf")) {
    Iterable<AnnotationItem> annotations = parser.getAnnotations();
    if (annotations == null) {
        System.out.println("Annotation extraction isn't supported.");
        return;
    }

    for (AnnotationItem annotation : annotations) {
        System.out.println("Type: " + annotation.getAnnotationType());
        System.out.println("Author: " + annotation.getAuthorName());
        System.out.println("Created/Modified: " + annotation.getTimestamp());
        System.out.println("Content: " + annotation.getValue());
    }
}
```

This metadata makes it easier to filter annotations by author or type, or sort them by time. To map annotations to a specific page, use the [getAnnotations(int pageIndex)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getAnnotations(int)) overload as shown above.

## Extract PDF Document Text Together with Annotations

Sometimes it's useful to get the document's text and its annotations in one pass, instead of calling `getAnnotations` separately. Call `setIncludeAnnotations(true)` on [TextOptions](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/TextOptions) and pass it to the [getText(TextOptions)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getText(com.groupdocs.parser.options.TextOptions)) method – the annotation text will be included in the extracted output alongside the regular document text.

### Example: Extract Text with Annotations in Java

```java
// Create an instance of Parser class
try (Parser parser = new Parser("MyDocument.pdf")) {
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

## Why Extract PDF Annotations?

Extracting PDF annotations is useful for:

*   Review workflows – collect reviewer comments and feedback from a document.
*   Collaboration – surface highlighted or noted sections without opening the PDF.
*   Auditing – track markup left on a document over time.

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
