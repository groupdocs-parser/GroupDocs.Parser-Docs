---
id: extract-text-from-microsoft-office-excel-spreadsheets
url: parser/java/extract-text-from-microsoft-office-excel-spreadsheets
title: Extract text from Microsoft Office Excel spreadsheets
weight: 1
description: "Learn how to extract text from Excel spreadsheets (.xls, .xlsx) using GroupDocs.Parser for Java. Includes examples for whole document extraction, sheet-by-sheet processing, raw mode extraction, and formatted text output."
keywords: "extract text from Excel, Excel text extraction, .xls parser, .xlsx parser, spreadsheet text extraction, GroupDocs.Parser Excel, Java Excel parser, sheet text extraction, Excel data mining"
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---

## Overview

This guide demonstrates how to extract text content from Microsoft Office Excel spreadsheets (.xls, .xlsx) using the GroupDocs.Parser for Java API. You'll learn different text extraction methods suitable for various document processing scenarios, from simple Excel text retrieval to sheet-by-sheet parsing.

## Extraction Methods Comparison

| Method | Use Case | Performance | Output Quality |
|--------|----------|-------------|----------------|
| **Whole Document** | Extract all text at once | Fast | Standard |
| **Sheet-by-Sheet** | Process individual worksheets | Medium | Standard |
| **Raw Mode** | High-speed bulk processing | Fastest | Lower formatting accuracy |
| **Formatted Text** | Preserve formatting (HTML/Markdown) | Slower | Highest |

## Method 1: Extract Text from Entire Spreadsheet

**When to use:** When you need all text content from the Excel workbook and don't need to distinguish between different worksheets.

To extract text from Microsoft Office Excel spreadsheets, the [getText](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getText()) method is used. This method retrieves content from the entire document.

### Steps:
1. Instantiate [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser) object for the initial spreadsheet
2. Call [getText](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getText()) method and obtain [TextReader](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/TextReader) object
3. Read text from the reader

{{< alert style="warning" >}}
[getText](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getText()) method returns *null* value if text extraction isn't supported for the document. For example, text extraction isn't supported for Zip archive. Therefore, for Zip archive [getText](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getText()) method returns *null*. For empty Microsoft Office Excel spreadsheets [getText](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getText()) method returns an empty [TextReader](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/TextReader) object ([readToEnd](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/TextReader#readToEnd()) method returns an empty string).
{{< /alert >}}

**Example:**
```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleXlsx)) {
    // Extract a text into the reader
    try (TextReader reader = parser.getText()) {
        // Print a text from the spreadsheet
        System.out.println(reader.readToEnd());
    }
}
```

## Method 2: Extract Text from Individual Sheets

**When to use:** When you need to process each Excel worksheet separately, maintain sheet organization, or perform sheet-specific data extraction.

This method uses [getText(int)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getText(int)) to extract text from specific sheets. Each worksheet is treated as a separate page of the document.

### Steps:
1. Instantiate [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser) object for the initial spreadsheet
2. Call [getDocumentInfo](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getDocumentInfo()) method and obtain [IDocumentInfo](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/IDocumentInfo) object with [getPageCount](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/IDocumentInfo#getPageCount()) method
3. Call [getText(int)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getText(int)) method with the sheet index and obtain [TextReader](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/TextReader) object
4. Read text from the reader

**Example:**
```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleXlsx)) {
    // Get the spreadsheet info
    IDocumentInfo spreadsheetInfo = parser.getDocumentInfo();

    // Iterate over sheets
    for (int p = 0; p < spreadsheetInfo.getPageCount(); p++) {
        // Print a sheet number
        System.out.println(String.format("Sheet %d/%d", p + 1, spreadsheetInfo.getPageCount()));

        // Extract a text into the reader
        try (TextReader reader = parser.getText(p)) {
            // Print a text from the spreadsheet
            System.out.println(reader.readToEnd());
        }
    }
}
```

## Method 3: High-Speed Raw Text Extraction

**When to use:** When processing large Excel files or many spreadsheets where parsing speed is more important than formatting accuracy. Suitable for Excel data mining, content indexing, or bulk document processing.

Raw mode increases text extraction performance by sacrificing formatting accuracy. Use [getText(TextOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getText(com.groupdocs.parser.options.TextOptions)) and [getText(int, TextOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getText(int,%20com.groupdocs.parser.options.TextOptions)) methods for raw mode extraction.

{{< alert style="warning" >}}
Some spreadsheets may have different sheet numbers in raw and accurate modes. Use [getRawPageCount](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/IDocumentInfo#getRawPageCount()) instead of [getPageCount](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/IDocumentInfo#getPageCount()) in raw mode.
{{< /alert >}}

### Steps:
1. Instantiate [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser) object for the initial spreadsheet
2. Instantiate [TextOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/TextOptions) object with *true* parameter
3. Call [getDocumentInfo](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getDocumentInfo()) method
4. Use [getRawPageCount](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/IDocumentInfo#getRawPageCount()) instead of [getPageCount](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/IDocumentInfo#getPageCount()) to avoid extra calculations
5. Call [getText(int, TextOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getText(int,%20com.groupdocs.parser.options.TextOptions)) method with the sheet index and obtain [TextReader](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/TextReader) object
6. Read text from the reader

**Example:**
```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleXlsx)) {
    // Get the spreadsheet info
    IDocumentInfo spreadsheetInfo = parser.getDocumentInfo();

    // Iterate over sheets
    for (int p = 0; p < spreadsheetInfo.getRawPageCount(); p++) {
        // Print a sheet number
        System.out.println(String.format("Sheet %d/%d", p + 1, spreadsheetInfo.getRawPageCount()));

        // Extract a text into the reader
        try (TextReader reader = parser.getText(p, new TextOptions(true))) {
            // Print a text from the spreadsheet
            System.out.println(reader.readToEnd());
        }
    }
}
```

## Method 4: Extract Formatted Text (HTML)

**When to use:** When you need to preserve the visual structure and formatting of Excel spreadsheet data, or when integrating extracted content into web applications or formatted reports.

GroupDocs.Parser allows extracting text from Microsoft Office Excel spreadsheets as HTML, Markdown and formatted plain text. For more details, see [Extract Formatted Text]({{< ref "parser/java/developer-guide/advanced-usage/working-with-text/working-with-formatted-text/extract-formatted-text-from-document.md" >}}).

### Steps:
1. Instantiate [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser) object for the initial spreadsheet
2. Call [getFormattedText(FormattedTextOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getFormattedText(com.groupdocs.parser.options.FormattedTextOptions)) method and obtain [TextReader](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/TextReader) object
3. Read text from the reader

**Example:**
```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleXlsx)) {
    // Extract a formatted text into the reader
    try (TextReader reader = parser.getFormattedText(new FormattedTextOptions(FormattedTextMode.Html))) {
        // Print a formatted text from the spreadsheet
        System.out.println(reader.readToEnd());
    }
}
```

## Supported Excel Formats

- **.xls** - Microsoft Excel 97-2003 Workbook
- **.xlsx** - Microsoft Excel Open XML Workbook

Other spreadsheet formats (XLSM, XLSB, XLT, XLTX, XLTM, ODS, OTS, CSV, XLA, XLAM, NUMBERS) are listed in the [Extract data from Microsoft Office Excel spreadsheets]({{< ref "parser/java/developer-guide/advanced-usage/extract-data-from-various-formats/extract-data-from-microsoft-office-excel/_index.md" >}}) article.

## Common Use Cases

### Enterprise Document Processing
- **Excel Data Mining**: Extract text for search indexing and content analysis
- **Spreadsheet Report Generation**: Convert Excel workbook data to text-based reports
- **Content Migration**: Move spreadsheet data between different document management systems

### Business Intelligence & Analytics
- **Text Analytics on Excel Files**: Analyze comments, notes, and text data within .xls/.xlsx spreadsheets
- **Compliance Document Processing**: Extract text content for regulatory reporting from Excel documents
- **Audit Trails**: Document text content changes over time in spreadsheet parsing workflows

### Integration Scenarios
- **Enterprise Search Systems**: Index Excel spreadsheet content for full-text search
- **Automated Data Pipelines**: Extract text as part of automated document processing workflows
- **Content Management Systems**: Process uploaded Excel files automatically in Java applications

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

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
