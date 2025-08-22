---
id: extract-metadata-from-pdf-documents
url: parser/net/extract-metadata-from-pdf-documents
title: Extract Metadata from PDF Documents in C# .NET
weight: 2
description: "Learn how to extract metadata from PDF files in C# using GroupDocs.Parser for .NET. Get document properties such as title, author, subject, creation date, and more."
keywords: extract PDF metadata C#, PDF metadata .NET, read PDF document properties C#, get PDF file information, GroupDocs.Parser metadata extraction
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---

# Extract Metadata from PDF Documents in C# .NET

PDF files often contain **metadata** such as the title, subject, author, creation date, and application used to generate the document. With **GroupDocs.Parser for .NET**, you can easily read these metadata properties programmatically using the [GetMetadata](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getmetadata) method.  

This guide shows how to extract metadata from PDF documents in C# step by step.

---

## What Metadata Can Be Extracted?

The `GetMetadata` method can return the following properties from a PDF document:

| Metadata Field | Description |
| --- | --- |
| **title** | Title of the PDF document |
| **subject** | Subject of the PDF document |
| **keywords** | Keywords associated with the document |
| **author** | Author of the document |
| **application** | Application that created the PDF |
| **application-version** | Version of the application |
| **created-time** | Date and time the PDF was created |
| **last-saved-time** | Date and time the PDF was last modified |

⚡ *Note: The available metadata depends on the PDF file itself. Not all fields may be present in every document.*

---

## How to Extract PDF Metadata in C#

Follow these steps to get metadata from a PDF document:

1. **Create a `Parser` object** and load the PDF file.  
2. **Call the `GetMetadata` method** to retrieve metadata items.  
3. **Iterate through the collection** and read metadata names and values.  

### Example: Extract PDF Metadata in C#
```csharp
// Load the PDF file
using (Parser parser = new Parser(filePath))
{
    // Extract metadata
    IEnumerable<MetadataItem> metadata = parser.GetMetadata();
  
    // Display metadata fields
    foreach (MetadataItem item in metadata)
    {
        Console.WriteLine($"{item.Name}: {item.Value}");
    }
}
```

{{< alert style="warning" >}}
[GetMetadata](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getmetadata) method returns *null* value if metadata extraction isn't supported for the document. For example, metadata extraction isn't supported for TXT files. Therefore, for TXT file [GetMetadata](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getmetadata) method returns *null*. If PDF document has no metadata, [GetMetadata](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getmetadata) method returns an empty collection.
{{< /alert >}}

## Why Extract PDF Metadata?

Extracting PDF metadata is useful for:
*   Document management – quickly find and categorize files by author, subject, or keywords.
*   Compliance checks – verify creation and modification dates.
*   Search indexing – improve search accuracy by including document properties.

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).