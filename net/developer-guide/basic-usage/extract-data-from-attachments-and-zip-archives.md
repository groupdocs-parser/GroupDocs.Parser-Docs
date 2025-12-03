---
id: extract-data-from-attachments-and-zip-archives
url: parser/net/extract-data-from-attachments-and-zip-archives
title: Extract data from attachments and ZIP archives
weight: 9
version: 23.5
description: "Learn how to extract text, images, and data from ZIP archives, PDF portfolios, email attachments, and Outlook storage files using GroupDocs.Parser for .NET in C#."
keywords: Extract data, PDF forms, ZIP-archived documents, extract from attachments, email attachments, PDF portfolios, container extraction C#
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
tags: csharp, parser, attachments, zip-archives, container, v23.5
---
It is easy to extract data, text, images and use any GroupDocs.Parser feature for ZIP-archived documents. The same feature allows to get attachments from  PDF and Emails and extract data from them.

# Extract data from attachments and ZIP archives

To extract documents from ZIP files and get attachments from containers simply call the [GetContainer](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getcontainer) method:

```csharp
IEnumerable<ContainerItem> GetContainer()

```

This method returns a collection of [ContainerItem](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/containeritem) objects:

| Member | Description |
| --- | --- |
| [Name](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/containeritem/properties/name) | The name of the item. |
| [Directory](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/containeritem/properties/directory) | The directory of the item. |
| [FilePath](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/containeritem/properties/filepath) | The full path of the item. |
| [Size](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/containeritem/properties/size) | The size of the item in bytes. |
| [Metadata](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/containeritem/properties/metadata) | The collection of item metadata. |
| Stream [OpenStream()](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/containeritem/methods/openstream) | Opens the stream of the item content. |
| Parser [OpenParser()](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/containeritem/methods/openparser) | Creates the Parser object for the item content. |
| Parser [OpenParser(LoadOptions)](https://reference.groupdocs.com/net/parser/groupdocs.parser.data.containeritem/openparser/methods/1) | Creates the Parser object for the item content with [LoadOptions](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/loadoptions). |
| Parser [OpenParser(LoadOptions, ParserSettings)](https://reference.groupdocs.com/net/parser/groupdocs.parser.data.containeritem/openparser/methods/2) | Creates the Parser object for the item content with [LoadOptions](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/loadoptions) and [ParserSettings](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/parsersettings). |

Container represents both container-only files (like zip archives, outlook storage) and documents with attachments (like emails, PDF Portfolios).

Here are the steps to extract an email text from outlook storage:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Call [GetContainer](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getcontainer) method and obtain collection of document container item objects;
*   Check if *collection* isn't *null* (container extraction is supported for the document);
*   Iterate through the collection and obtain [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object to extract a text.

The following example shows how to extract text from ZIP archive files with comprehensive error handling:

```csharp
using GroupDocs.Parser;
using GroupDocs.Parser.Data;
using GroupDocs.Parser.Exceptions;
using System;
using System.Collections.Generic;
using System.IO;

try
{
    // Create an instance of Parser class
    using (Parser parser = new Parser(filePath))
    {
        // Extract attachments from the container
        IEnumerable<ContainerItem> attachments = parser.GetContainer();
        
        // Check if container extraction is supported
        if (attachments == null)
        {
            Console.WriteLine("Container extraction isn't supported for this file format.");
            return;
        }
        
        // Iterate over container items (ZIP entries, attachments, etc.)
        int itemIndex = 0;
        foreach (ContainerItem item in attachments)
        {
            itemIndex++;
            Console.WriteLine($"\n=== Item {itemIndex}: {item.FilePath} ===");
            Console.WriteLine($"Size: {item.Size} bytes");
            Console.WriteLine($"Directory: {item.Directory}");
            
            try
            {
                // Create Parser object for the container item content
                using (Parser attachmentParser = item.OpenParser())
                {
                    // Extract text from the item
                    using (TextReader reader = attachmentParser.GetText())
                    {
                        if (reader == null)
                        {
                            Console.WriteLine("Text extraction is not supported for this item.");
                        }
                        else
                        {
                            string text = reader.ReadToEnd();
                            if (string.IsNullOrEmpty(text))
                            {
                                Console.WriteLine("Item contains no text.");
                            }
                            else
                            {
                                Console.WriteLine($"Extracted {text.Length} characters of text.");
                                // Uncomment to see full text: Console.WriteLine(text);
                            }
                        }
                    }
                }
            }
            catch (UnsupportedDocumentFormatException)
            {
                Console.WriteLine("Document format is not supported for this item.");
            }
            catch (ParserException ex)
            {
                Console.WriteLine($"Error parsing item: {ex.Message}");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Unexpected error: {ex.Message}");
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
    Console.WriteLine($"Error: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Unexpected error: {ex.Message}");
}
```

## More resources

### Advanced usage topics

To learn more how to work with attachments and ZIP archives, please refer the [advanced help section]({{< ref "parser/net/developer-guide/advanced-usage/working-with-zip-archives-and-attachments/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to extract text, metadata and images from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
