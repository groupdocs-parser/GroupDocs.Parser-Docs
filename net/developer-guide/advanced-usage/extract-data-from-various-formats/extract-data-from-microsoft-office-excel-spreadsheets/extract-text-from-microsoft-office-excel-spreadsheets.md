---
id: extract-text-from-microsoft-office-excel-spreadsheets
url: parser/net/extract-text-from-microsoft-office-excel-spreadsheets
title: Extract text from Microsoft Office Excel spreadsheets
weight: 1
description: "This article explains that how to extract text from Microsoft Office Excel (.xls, .xlsx) spreadsheets."
keywords: extract text, extract text from Microsoft Office Excel, .xls, .xlsx
productName: GroupDocs.Parser for .NET
hideChildren: False
---
To extract a text from Microsoft Office Excel spreadsheets [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) and [GetText(pageIndex)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/2) method is used. These methods allow to extract a text from the entire document or a text from the selected page.
Here are the steps to extract a text from Microsoft Office Excel spreadsheets:
*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial spreadsheet;
*   Call [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object;
*   Read a text from *reader*.

{{< alert style="warning" >}}
[GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method returns *null* value if text extraction isn't supported for the document. For example, text extraction isn't supported for Zip archive. Therefore, for Zip archive [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method returns *null*. For empty Microsoft Office Excel spreadsheets [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method returns an empty [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object ([reader.ReadToEnd](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader.readtoend?view=netframework-2.0) method returns an empty string).
{{< /alert >}}

The following example demonstrates how to extract a text from Microsoft Office Excel spreadsheets:
```csharp
// Create an instance of Parser class
using(Parser parser = new Parser(filePath))
{
    // Extract a text into the reader
    using(TextReader reader = parser.GetText())
    {
        // Print a text from the spreadsheet
        Console.WriteLine(reader.ReadToEnd());
    }
}
```

Here are the steps to extract a text from the sheet of Microsoft Office Excel spreadsheet:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial spreadsheet;
*   Call [GetDocumentInfo](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getdocumentinfo) method and obtain [IDocumentInfo](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo) object with [page count](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/pagecount);
*   Call [GetText(pageIndex)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/2) method with the sheet index and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object;
*   Read a text from *reader*.

The following example shows how to extract a text from the sheet of Microsoft Office Excel spreadsheet:

```csharp
// Create an instance of Parser class
using(Parser parser = new Parser(filePath))
{
    // Get the document info
    IDocumentInfo documentInfo = parser.GetDocumentInfo();
   
    // Iterate over sheets
    for(int p = 0; p < documentInfo.PageCount; p++)
    {
        // Print a sheet number 
        Console.WriteLine(string.Format("Page {0}/{1}", p + 1, documentInfo.PageCount));
   
        // Extract a text into the reader
        using(TextReader reader = parser.GetText(p))
        {
            // Print a text from the spreadsheet sheet
            Console.WriteLine(reader.ReadToEnd());
        }
    }
}
```

Raw mode allows to increase the speed of text extraction due to poor formatting accuracy. [GetText(TextOptions)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/1) and [GetText(pageIndex, TextOptions)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/3) methods are used to extract a text in raw mode.

{{< alert style="warning" >}}
Some spreadsheets may have different sheet numbers in raw and accurate modes. Use [IDocumentInfo.RawPageCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/rawpagecount) instead of [IDocumentInfo.PageCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/pagecount) in raw mode.
{{< /alert >}}

Here are the steps to extract a raw text from the sheet of Microsoft Office Excel spreadsheet:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial spreadsheet;
*   Instantiate [TextOptions](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/textoptions) object with *true* parameter;
*   Call [GetDocumentInfo](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getdocumentinfo) method;
*   Use [RawPageCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/rawpagecount) instead of [PageCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/pagecount) to avoid extra calculations;
*   Call [GetText(pageIndex, TextOptions)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/3) method with the sheet index and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object;
*   Read a text from *reader*.

The following example shows how to extract a raw text from the sheet of Microsoft Office Excel spreadsheet:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser(filePath))
{
    // Get the document info
    IDocumentInfo documentInfo = parser.GetDocumentInfo();
    // Check if the document has pages
    if (documentInfo == null || documentInfo.RawPageCount == 0)
    {
        Console.WriteLine("Document hasn't pages.");
        return;
    }
    // Iterate over sheets
    for (int p = 0; p < documentInfo.RawPageCount; p++)
    {
        // Print a sheet number 
        Console.WriteLine(string.Format("Page {0}/{1}", p + 1, documentInfo.RawPageCount));
        // Extract a text into the reader
        using (TextReader reader = parser.GetText(p, new TextOptions(true)))
        {
            // Print a text from the spreadsheet sheet
            Console.WriteLine(reader.ReadToEnd());
        }
    }
}
```

GroupDocs.Parser also allows to extract a text from Microsoft Office Excel spreadsheets as HTML, Markdown and formatted plain text. For more details, see [Extract Formatted Text]({{< ref "parser/net/developer-guide/advanced-usage/working-with-text/working-with-formatted-text/extract-formatted-text-from-document.md" >}}).

Here are the steps to extract a text from Microsoft Office Excel spreadsheet as HTML:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial spreadsheet;
*   Call [GetFormattedText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getformattedtext) method and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object;
*   Read a text from *reader*.

The following example shows how to extract a text from Microsoft Office Excel spreadsheet as HTML:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser(filePath))
{
    // Extract a formatted text into the reader
    using (TextReader reader = parser.GetFormattedText(new FormattedTextOptions(FormattedTextMode.Html)))
    {
        // Print a formatted text from the sheet
        Console.WriteLine(reader.ReadToEnd());
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