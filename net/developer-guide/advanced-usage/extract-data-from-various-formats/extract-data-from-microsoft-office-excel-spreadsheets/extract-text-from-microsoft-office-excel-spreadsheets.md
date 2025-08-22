---
id: extract-text-from-microsoft-office-excel-spreadsheets
url: parser/net/extract-text-from-microsoft-office-excel-spreadsheets
title: Extract text from Microsoft Office Excel spreadsheets
weight: 1
description: "Learn how to extract text from Excel spreadsheets (.xls, .xlsx) using GroupDocs.Parser for .NET. Includes examples for whole document extraction, sheet-by-sheet processing, raw mode extraction, and formatted text output."
keywords: "extract text from Excel, Excel text extraction, .xls parser, .xlsx parser, spreadsheet text extraction, GroupDocs.Parser Excel, C# Excel parser, sheet text extraction, Excel data mining"
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---

## Overview

This guide demonstrates how to extract text content from Microsoft Office Excel spreadsheets (.xls, .xlsx) using the GroupDocs.Parser for .NET API. You'll learn different text extraction methods suitable for various document processing scenarios, from simple Excel text retrieval to advanced sheet-by-sheet parsing operations.

## Extraction Methods Comparison

| Method | Use Case | Performance | Output Quality |
|--------|----------|-------------|----------------|
| **Whole Document** | Extract all text at once | Fast | Standard |
| **Sheet-by-Sheet** | Process individual worksheets | Medium | Standard |
| **Raw Mode** | High-speed bulk processing | Fastest | Lower formatting accuracy |
| **Formatted Text** | Preserve formatting (HTML/Markdown) | Slower | Highest |

## Method 1: Extract Text from Entire Spreadsheet

**When to use:** When you need all text content from the Excel workbook and don't need to distinguish between different worksheets for your document parsing workflow.

To extract text from Microsoft Office Excel spreadsheets using the .NET parser library, the [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method is used. This text extraction API method retrieves content from the entire document.

### Steps:
1. Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial spreadsheet
2. Call [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object
3. Read text from the reader

{{< alert style="warning" >}}
[GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method returns *null* value if text extraction isn't supported for the document. For example, text extraction isn't supported for Zip archive. Therefore, for Zip archive [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method returns *null*. For empty Microsoft Office Excel spreadsheets [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method returns an empty [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object ([reader.ReadToEnd](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader.readtoend?view=netframework-2.0) method returns an empty string).
{{< /alert >}}

**Example:**
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

## Method 2: Extract Text from Individual Sheets

**When to use:** When you need to process each Excel worksheet separately for your C# spreadsheet parser application, maintain sheet organization, or perform sheet-specific data extraction operations.

This Excel parsing method uses [GetText(pageIndex)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/2) to extract text from specific sheets. Each worksheet is treated as a separate page in the document parsing process.

### Steps:
1. Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial spreadsheet
2. Call [GetDocumentInfo](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getdocumentinfo) method and obtain [IDocumentInfo](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo) object with [page count](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/pagecount)
3. Call [GetText(pageIndex)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/2) method with the sheet index and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object
4. Read text from the reader

**Example:**
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

## Method 3: High-Speed Raw Text Extraction

**When to use:** When processing large Excel files or multiple spreadsheets where parsing speed is more important than formatting accuracy. Ideal for Excel data mining, content indexing, or bulk document processing scenarios in enterprise applications.

Raw mode increases text extraction performance by sacrificing formatting accuracy in the .NET parser. Use [GetText(TextOptions)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/1) and [GetText(pageIndex, TextOptions)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/3) methods for high-speed raw mode extraction.

{{< alert style="warning" >}}
Some spreadsheets may have different sheet numbers in raw and accurate modes. Use [IDocumentInfo.RawPageCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/rawpagecount) instead of [IDocumentInfo.PageCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/pagecount) in raw mode.
{{< /alert >}}

### Steps:
1. Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial spreadsheet
2. Instantiate [TextOptions](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/textoptions) object with *true* parameter
3. Call [GetDocumentInfo](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getdocumentinfo) method
4. Use [RawPageCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/rawpagecount) instead of [PageCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/pagecount) to avoid extra calculations
5. Call [GetText(pageIndex, TextOptions)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/3) method with the sheet index and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object
6. Read text from the reader

**Example:**
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

## Method 4: Extract Formatted Text (HTML)

**When to use:** When you need to preserve the visual structure and formatting of Excel spreadsheet data using the .NET document parser, or when integrating extracted content into web applications or formatted reports.

The GroupDocs.Parser text extraction library allows extracting text from Microsoft Office Excel spreadsheets as HTML, Markdown, and formatted plain text. For more details, see [Extract Formatted Text]({{< ref "parser/net/developer-guide/advanced-usage/working-with-text/working-with-formatted-text/extract-formatted-text-from-document.md" >}}).

### Steps:
1. Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial spreadsheet
2. Call [GetFormattedText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getformattedtext) method and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object
3. Read text from the reader

**Example:**
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

## Supported Excel Formats

- **.xls** - Microsoft Excel 97-2003 Workbook
- **.xlsx** - Microsoft Excel Open XML Workbook

## Common Use Cases

### Enterprise Document Processing
- **Excel Data Mining**: Extract text for search indexing and content analysis using C# parser methods
- **Spreadsheet Report Generation**: Convert Excel workbook data to text-based reports
- **Content Migration**: Move spreadsheet data between different document management systems

### Business Intelligence & Analytics
- **Text Analytics on Excel Files**: Analyze comments, notes, and text data within .xls/.xlsx spreadsheets
- **Compliance Document Processing**: Extract text content for regulatory reporting from Excel documents  
- **Audit Trails**: Document text content changes over time in spreadsheet parser workflows

### Integration Scenarios
- **Enterprise Search Systems**: Index Excel spreadsheet content for full-text search capabilities
- **Automated Data Pipelines**: Extract text as part of automated document processing workflows
- **Content Management Systems**: Process uploaded Excel files automatically using the .NET parsing API

## Performance Considerations

### File Size Impact
- **Small files** (< 1MB): All methods perform similarly
- **Medium files** (1-10MB): Raw mode provides noticeable speed improvement
- **Large files** (> 10MB): Raw mode recommended for bulk processing

### Memory Usage
- Sheet-by-sheet processing uses less memory than whole document extraction
- Raw mode is more memory-efficient for large files
- Consider processing sheets individually for very large workbooks

## Troubleshooting

### Common Issues

**Null TextReader Response**
- Verify the file format is supported (.xls, .xlsx)
- Check if the file is corrupted or password-protected
- Ensure the file path is correct and accessible

**Empty Text Output**
- Confirm the spreadsheet contains text data (not just numbers/formulas)
- Check if the sheets contain visible content
- Verify the file isn't completely empty

**Performance Issues**
- Use raw mode for large files or bulk processing
- Process sheets individually instead of extracting the entire document
- Consider file size limitations in your environment

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).