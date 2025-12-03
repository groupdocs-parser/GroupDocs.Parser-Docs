---
id: get-document-info
url: parser/net/get-document-info
title: Get document info
weight: 2
version: 23.5
description: "Learn how to get basic document information including file type, page count, and file size using GroupDocs.Parser for .NET. Get document properties in C#."
keywords: Document Info, Parser, C#, CSharp, .Net, dotNet, get file type, get page count, document properties, file information
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
tags: csharp, parser, document-info, file-properties, v23.5
---
GroupDocs.Parser provides the functionality to get the basic document info by the [GetDocumentInfo](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getdocumentinfo) method:

```csharp
IDocumentInfo GetDocumentInfo();

```

[IDocumentInfo](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo) interface has the following members:

| Member | Description |
| --- | --- |
| [FileType](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/filetype) | The document type. |
| [PageCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/pagecount) | The total number of document pages. |
| [Size](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/size) | The size of the document in bytes. |

Here are the steps to get document info:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Call [GetDocumentInfo](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getdocumentinfo) method and obtain the object with [IDocumentInfo](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo) interface;
*   Call properties such as [FileType](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/filetype), [PageCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/pagecount) or [Size](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/size).

The following example shows how to get document info with error handling:

```csharp
using GroupDocs.Parser;
using GroupDocs.Parser.Options;
using GroupDocs.Parser.Exceptions;
using System;
using System.IO;

try
{
    // Create an instance of Parser class
    using (Parser parser = new Parser(filePath))
    {
        // Get the document info
        IDocumentInfo info = parser.GetDocumentInfo();
        
        if (info == null)
        {
            Console.WriteLine("Unable to retrieve document information.");
            return;
        }
        
        // Display document information
        Console.WriteLine("=== Document Information ===");
        Console.WriteLine($"File Type: {info.FileType}");
        Console.WriteLine($"Page Count: {info.PageCount}");
        Console.WriteLine($"File Size: {info.Size:N0} bytes ({info.Size / 1024.0:F2} KB)");
        
        // Additional info if available
        if (info is ITextDocumentInfo textInfo)
        {
            if (textInfo.Encoding != null)
            {
                Console.WriteLine($"Encoding: {textInfo.Encoding.EncodingName}");
            }
        }
    }
}
catch (FileNotFoundException)
{
    Console.WriteLine($"Error: File not found at path '{filePath}'");
}
catch (ParserException ex)
{
    Console.WriteLine($"Error getting document info: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Unexpected error: {ex.Message}");
}
```

## More resources

### Advanced usage topics

To learn more about document data extraction features and get familiar how to extract text, images, forms and more, please refer to the [advanced usage section]({{< ref "parser/net/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).