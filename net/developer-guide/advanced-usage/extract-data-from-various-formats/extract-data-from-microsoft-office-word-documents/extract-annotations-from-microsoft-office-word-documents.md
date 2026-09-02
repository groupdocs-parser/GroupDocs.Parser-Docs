---
id: extract-annotations-from-microsoft-office-word-documents
url: parser/net/extract-annotations-from-microsoft-office-word-documents
title: Extract Annotations from Microsoft Office Word documents
weight: 2
description: "This article explains that how to extract annotations from Microsoft Office Word (.doc, .docx) documents"
keywords: extract annotations, extract annotations from Microsoft Office Word, .doc, .docx
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---

Microsoft Office Word documents (.doc, .docx) often contain **annotations** – comments, tracked changes, or embedded notes left by reviewers. With **GroupDocs.Parser for .NET**, you can easily read these annotations programmatically using the [GetAnnotations](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/getannotations/#getannotations) method.

This guide shows how to extract annotations from Microsoft Office Word documents in C# step by step.
---
## What Are Word Annotations?

Annotations are comments, tracked changes, or other embedded notes that reviewers attach to a Word (.doc, .docx) document during a review pass. The `GetAnnotations` method reads this markup and returns it as a collection of `AnnotationItem` objects, each exposing a `Value` with the annotation's text, along with metadata such as the author and timestamp.

The method has two overloads:

| Overload | Description |
| --- | --- |
| **GetAnnotations()** | Extracts annotations from the whole Word document |
| **GetAnnotations(int pageIndex)** | Extracts annotations from a specific page (zero-based index) |

⚡ *Note: Annotation extraction is supported for electronic text documents, including the Open XML format (.docx), the legacy binary format (.doc), RTF, and TXT.*
---
## How to Extract Annotations from a Word Document in C#

Follow these steps to get annotations from a .doc or .docx file:

1. **Create a `Parser` object** and load the Word document.
2. **Call the `GetAnnotations` method** to retrieve the annotation collection.
3. **Iterate through the collection** and read each annotation's value.

### Example: Extract Word Annotations in C#

```csharp
using (Parser parser = new Parser(@"MyDocument.docx"))
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
[GetAnnotations](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/getannotations/#getannotations) method returns *null* if annotation extraction isn't supported for the document. If the Word document has no comments, [GetAnnotations](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/getannotations/#getannotations) method returns an empty collection.
{{< /alert >}}

## Extract Word Text Together with Annotations

Sometimes it's useful to get the document's text and its comments in one pass, instead of calling `GetAnnotations` separately. Set `IncludeAnnotations` to `true` on `TextOptions` and pass it to the `GetText` method – the annotation text will be included in the extracted output alongside the regular document text.

### Example: Extract Text with Annotations in C#

```csharp
using (Parser parser = new Parser(@"MyDocument.docx"))
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

## Reading Word Annotation Metadata

Each `AnnotationItem` also exposes metadata that helps trace who left a comment in a document and when:

| Property | Type | Description |
| --- | --- | --- |
| `AnnotationType` | `string` | The annotation type. |
| `PageIndex` | `int` | Zero-based index of the page where the annotation is located. |
| `Timestamp` | `DateTime?` | Date and time when the annotation was created or last modified. |
| `AuthorName` | `string` | Name of the author who created or last modified the annotation. |
| `AuthorInitials` | `string` | Initials of the author who created or last modified the annotation. |

### Example: Reading Annotation Metadata in C#

```csharp
using (Parser parser = new Parser(@"MyDocument.docx"))
{
    IEnumerable<AnnotationItem> annotations = parser.GetAnnotations();

    foreach (AnnotationItem annotation in annotations)
    {
        Console.WriteLine($"Page: {annotation.PageIndex}, Author: {annotation.AuthorName} ({annotation.AuthorInitials})");
        Console.WriteLine($"Created/Modified: {annotation.Timestamp}, Type: {annotation.AnnotationType}");
        Console.WriteLine($"Comment: {annotation.Value}");
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
