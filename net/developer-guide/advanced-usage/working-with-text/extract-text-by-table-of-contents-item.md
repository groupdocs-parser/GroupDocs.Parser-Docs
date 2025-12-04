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

The following example shows how to extract text by table of contents items:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser(filePath))
{
    // Get table of contents
    IEnumerable<TocItem> toc = parser.GetToc();
    
    // Check if TOC extraction is supported
    if (toc == null)
    {
        Console.WriteLine("Table of contents extraction isn't supported");
        return;
    }
    
    // Iterate over items
    foreach (TocItem i in toc)
    {
        // Check if page index has a value
        if (i.PageIndex == null)
        {
            continue;
        }
        // Extract a page text
        using (TextReader reader = i.ExtractText())
        {
            Console.WriteLine(reader == null ? "No text" : reader.ReadToEnd());
        }
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