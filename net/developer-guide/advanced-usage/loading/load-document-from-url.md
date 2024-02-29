---
id: load-document-from-url
url: parser/net/load-document-from-url
title: Load document from url
weight: 3
description: "Learn how to Load document from url."
keywords: Load document from url, extract data
productName: GroupDocs.Parser for .NET
hideChildren: False
---

To avoid the overhead of saving documents to the disk, GroupDocs.Parser enables to extract data from url directly.

The following example shows how to load the document from the url:

```csharp
// Create an instance of Parser class with the url
using (Parser parser = new Parser(url))
{
    // Extract a text into the reader
    using (TextReader reader = parser.GetText())
    {
        // Print a text from the document
        // If text extraction isn't supported, a reader is null
        Console.WriteLine(reader == null ? "Text extraction isn't supported" : reader.ReadToEnd());
    }
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).