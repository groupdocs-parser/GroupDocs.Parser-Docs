---
id: extract-metadata-from-documents
url: parser/net/extract-metadata-from-documents
title: Extract metadata from documents
weight: 7
description: "This article shows how to extract metadata with GroupDocs.Parser from documents of various formats: PDF, Emails, Ebooks, Microsoft Office: Word (DOC, DOCX), PowerPoint (PPT, PPTX), Excel (XLS, XLSX), LibreOffice formats and many others."
keywords: Extract metadata, extract basic metadata, extract metadata from image
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---
GroupDocs.Parser allows to extract basic metadata from documents of various formats: PDF, Emails, Ebooks, Microsoft Office: Word (DOC, DOCX), PowerPoint (PPT, PPTX), Excel (XLS, XLSX), LibreOffice formats and many others (see full list at [supported document formats]({{< ref "parser/net/getting-started/supported-document-formats.md" >}}) article).

## Extract metadata from documents

To extract metadata from documents simply call the [GetMetadata](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getmetadata) method:

```csharp
IEnumerable<MetadataItem> GetMetadata();

```

This method returns a collection of [MetadataItem](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/metadataitem) objects with following members:

| Member | Description |
| --- | --- |
| [Name](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/metadataitem/properties/name) | The name of the metadata item |
| [Value](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/metadataitem/properties/value) | The value of the metadata item |

Here are the steps to extract metadata from the document:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Call [GetMetadata](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getmetadata) method and obtain collection of document metadata objects;
*   Check if *collection* isn't null (metadata extraction is supported for the document);
*   Iterate through the collection and get metadata names and values.

The following example shows how to extract metadata from a document:

```csharp
// Create an instance of Parser class
using(Parser parser = new Parser(filePath))
{
    // Extract metadata from the document
    IEnumerable<MetadataItem> metadata = parser.GetMetadata();
    // Check if metadata extraction is supported
    if(metadata == null)
    {
        Console.WriteLine("Metatada extraction isn't supported");
    }

    // Iterate over metadata items
    foreach(MetadataItem item in metadata)
    {
        // Print an item name and value
        Console.WriteLine(string.Format("{0}: {1}", item.Name, item.Value));
    }
}

```

## More resources

### Advanced usage topics

To learn more about document data extraction features and get familiar how to extract text, images, forms and more, please refer to the [advanced usage section]({{< ref "parser/net/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online metadata extractor App

Along with full featured .NET library we provide simple, but powerfull free APPs.

You are welcome to extract metadata from your documents with our free online [GroupDocs Parser App](https://products.groupdocs.app/parser).