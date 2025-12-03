---
id: extract-text-by-table-of-contents-item
url: parser/net/extract-text-by-table-of-contents-item
title: Extract text by table of contents item
weight: 9
version: 23.5
description: "Learn how to extract text by specific table of contents items using GroupDocs.Parser for .NET. Extract chapter content from Word documents, PDFs, and eBooks by TOC item in C#."
keywords: extract text, extract text by table of contents item, extract chapter text, TOC item extraction, extract by heading C#
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
tags: csharp, parser, toc, table-of-contents, chapter-extraction, v23.5
---
GroupDocs.Parser provides the functionality to extract a text by an item of table of contents. This feature is supported for Word Processing, PDF, ePUB and CHM documents (for more details, see [Supported Document Formats]({{< ref "parser/net/getting-started/supported-document-formats.md" >}})).

Text is extracted by [TocItem.ExtractText](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/tocitem/methods/extracttext) method:

```csharp
// Get the first item of table of contents
TocItem tocItem = parser.GetToc().First();

// Print the text of the chapter
using (TextReader reader = tocItem.ExtractText())
{
    Console.WriteLine("----");
    Console.WriteLine(reader.ReadToEnd());
}
```

This method returns a text from the chapter to which table of contents item refers (without sub-chapters). For example, "Heading 1.2" from the page

![](/parser/net/images/extract-text-by-table-of-contents-item.png)

returns the following text:

![](/parser/net/images/extract-text-by-table-of-contents-item_1.png)

"Heading 2" from the page:

![](/parser/net/images/extract-text-by-table-of-contents-item_2.png)

returns the following text:

![](/parser/net/images/extract-text-by-table-of-contents-item_3.png)

{{< alert style="warning" >}}
[InvalidOperationException](https://docs.microsoft.com/en-us/dotnet/api/system.invalidoperationexception?view=netframework-2.0) is thrown if [tocItem.PageIndex](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/tocitem/properties/pageindex) is *null*.
{{< /alert >}}

Here are the steps to extract a text by an item of table of contents:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Call [GetToc](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettoc) method and obtain the collection of [TocItem](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/tocitem) objects;
*   Check if *collection* isn't *null* (table of contents extraction is supported for the document);
*   Iterate through the collection and extract a text by [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/tocitem/methods/gettext) method.  

The following example shows how to extract text by table of contents items with error handling:

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
        // Get table of contents
        IEnumerable<TocItem> tocItems = parser.GetToc();
        
        // Check if TOC extraction is supported
        if (tocItems == null)
        {
            Console.WriteLine("Table of contents extraction isn't supported for this document format.");
            return;
        }
        
        var tocList = new List<TocItem>(tocItems);
        
        if (tocList.Count == 0)
        {
            Console.WriteLine("Document has no table of contents items.");
            return;
        }
        
        Console.WriteLine($"Found {tocList.Count} TOC item(s)\n");
        
        // Iterate over items
        foreach (TocItem tocItem in tocList)
        {
            Console.WriteLine($"=== {tocItem.Text} ===");
            Console.WriteLine($"Depth: {tocItem.Depth}");
            
            // Check if page index is available
            if (tocItem.PageIndex == null)
            {
                Console.WriteLine("Page index is not available for this item.\n");
                continue;
            }
            
            Console.WriteLine($"Page: {tocItem.PageIndex.Value + 1}");
            
            try
            {
                // Extract text for this chapter/item
                using (TextReader reader = tocItem.ExtractText())
                {
                    if (reader != null)
                    {
                        string chapterText = reader.ReadToEnd();
                        if (!string.IsNullOrEmpty(chapterText))
                        {
                            Console.WriteLine($"Text length: {chapterText.Length} characters");
                            Console.WriteLine("---");
                            // Display first 200 characters as preview
                            Console.WriteLine(chapterText.Length > 200 
                                ? chapterText.Substring(0, 200) + "..." 
                                : chapterText);
                        }
                        else
                        {
                            Console.WriteLine("No text found for this item.");
                        }
                    }
                    else
                    {
                        Console.WriteLine("Text extraction returned null.");
                    }
                }
            }
            catch (InvalidOperationException ex)
            {
                Console.WriteLine($"Error: {ex.Message}");
                Console.WriteLine("This usually means the page index is null.");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error extracting text: {ex.Message}");
            }
            
            Console.WriteLine();
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

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).