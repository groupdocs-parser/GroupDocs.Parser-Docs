---
id: extract-annotations-from-pdf-documents
url: parser/net/extract-annotations-from-pdf-documents
title: Extract Annotations from PDF Documents in C# .NET
weight: 2
description: "Learn how to extract annotations from PDF files in C# using GroupDocs.Parser for .NET."
keywords: extract PDF annotations C#, PDF annotations .NET, read PDF document annotations C#, get PDF file annotations, GroupDocs.Parser annotations extraction
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---

PDF files often contain **annotations** – comments, notes, highlights, and other markup added by reviewers. With **GroupDocs.Parser for .NET**, you can easily read these annotations programmatically using the [GetAnnotations](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/getannotations/#getannotations) method.

This guide shows how to extract annotations from PDF documents in C# step by step.

---

## What Are PDF Annotations?

Annotations are notes, comments, and markup that reviewers add on top of a PDF document's content – for example, sticky notes, text comments, or highlighted remarks. The `GetAnnotations` method reads this markup and returns it as a collection of `AnnotationItem` objects, each exposing a `Value` with the annotation's text.

The method has two overloads:

| Overload | Description |
| --- | --- |
| **GetAnnotations()** | Extracts annotations from the whole document |
| **GetAnnotations(int pageIndex)** | Extracts annotations from a specific page (zero-based index) |

⚡ *Note: Annotation support depends on the document format. Not every file format allows annotations.*

---

## How to Extract PDF Annotations in C#

Follow these steps to get annotations from a PDF document:

1. **Create a `Parser` object** and load the PDF file.
2. **Call the `GetAnnotations` method** to retrieve the annotation collection.
3. **Iterate through the collection** and read each annotation's value.

### Example: Extract PDF Annotations in C#

```csharp
using (Parser parser = new Parser(@"MyDocument.pdf"))
{
    // Extract annotations
    IEnumerable<AnnotationItem> annotations = parser.GetAnnotations();

    // Display annotations
    foreach (AnnotationItem item in annotations)
    {
        Console.WriteLine(item.Value);
    }
}
```

{{< alert style="warning" >}}
[GetAnnotations](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/getannotations/#getannotations) method returns *null* if annotation extraction isn't supported for the document. If the PDF document has no annotations, [GetAnnotations](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/getannotations/#getannotations) method returns an empty collection.
{{< /alert >}}

## Additional Annotation Metadata

Starting with version 26.8, the `AnnotationItem` class exposes extra metadata that gives more context about each annotation:

| Property | Type | Description |
| --- | --- | --- |
| `AnnotationType` | `string` | The annotation type. |
| `PageIndex` | `int` | Zero-based index of the page (or slide) where the annotation is located. |
| `Timestamp` | `DateTime?` | Date and time when the annotation was created or last modified. |
| `AuthorName` | `string` | Name of the author who created or last modified the annotation. |
| `AuthorInitials` | `string` | Initials of the author who created or last modified the annotation. |

### Example: Read Annotation Metadata in C#

```csharp
using (Parser parser = new Parser(@"MyDocument.pdf"))
{
    foreach (AnnotationItem annotation in parser.GetAnnotations())
    {
        Console.WriteLine($"Type: {annotation.AnnotationType}");
        Console.WriteLine($"Author: {annotation.AuthorName} ({annotation.AuthorInitials})");
        Console.WriteLine($"Created/Modified: {annotation.Timestamp}");
        Console.WriteLine($"Page: {annotation.PageIndex}");
        Console.WriteLine($"Content: {annotation.Value}");
    }
}
```

This metadata makes it easier to filter annotations by author or type, sort them by time, or map them back to a specific page without extra lookups.

## Extract PDF Document Text Together with Annotations

Sometimes it's useful to get the document's text and its annotations in one pass, instead of calling `GetAnnotations` separately. Set `IncludeAnnotations` to `true` on `TextOptions` and pass it to the `GetText` method – the annotation text will be included in the extracted output alongside the regular document text.

### Example: Extract Text with Annotations in C#

```csharp
using (Parser parser = new Parser(@"MyDocument.pdf"))
{
    TextOptions options = new TextOptions
    {
        IncludeAnnotations = true
    };
    using (TextReader reader = parser.GetText(options))
    {
        string text = reader.ReadToEnd();
        Console.WriteLine(text);
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
