---
id: extract-tables-from-document
url: parser/net/extract-tables-from-document
title: Extract tables from document
weight: 4
version: 23.5
description: "Learn how to extract tables from documents including Excel spreadsheets, Word documents, and PDFs using GroupDocs.Parser for .NET. Complete guide with code examples for extract tables from Excel C# scenarios."
keywords: extract tables, extract tables from document, extract tables from Excel C#, Excel table extraction, extract tables from XLSX, C# table parser
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
tags: csharp, parser, table-extraction, excel-tables, v23.5
---

GroupDocs.Parser provides the functionality to extract tables from documents including **Excel spreadsheets (XLS, XLSX)**, Word documents, PDFs, and other supported formats using the [GetTables(PageTableAreaOptions)](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/methods/gettables) method.

## Extract Tables from Excel C#

To **extract tables from Excel C#** applications, GroupDocs.Parser supports extracting structured table data from Excel spreadsheets. This is especially useful for processing data-rich Excel files programmatically.

```csharp
IEnumerable<PageTableArea> GetTables(PageTableAreaOptions options);
```

This method returns a collection of [PageTableArea](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagetablearea) object:

| Member                                                       | Description                                     |
| ------------------------------------------------------------ | ----------------------------------------------- |
| [Rectangle](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagearea/properties/rectangle) | The rectangular area that bounds text area.     |
| [Page](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagearea/properties/page) | The page information (page index and page size) |
| [RowCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagetablearea/properties/rowcount) | The total number of the table rows.             |
| [ColumnCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagetablearea/properties/columncount) | The total number of the table columns.          |
| PageTableAreaCell [Item](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagetablearea/properties/item) | The table cell by row and column indexes.       |
| double [GetRowHeight(int)](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagetablearea/methods/getrowheight) | The the row height.                             |
| double [GetColumnWidth(int)](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagetablearea/methods/getcolumnwidth) | Returns the column width.                       |

[GetTables(PageTableAreaOptions)](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/methods/gettables) accepts [PageTableAreaOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/pagetableareaoptions) object that contains [TemplateTableLayout](https://reference.groupdocs.com/parser/net/groupdocs.parser.templates/templatetablelayout) object with table layout (see [this article]({{< ref "parser/net/developer-guide/advanced-usage/working-with-templates.md" >}}) for more details).

### Basic Steps

Here are the basic steps to extract tables from documents:

1. Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the document (Excel, Word, PDF, etc.);
2. Check if the document supports **table extraction** using `Features.Tables` property;
3. Call [GetTables(PageTableAreaOptions)](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/methods/gettables) method and obtain collection of [PageTableArea](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagetablearea) objects;
4. Iterate through the collection and access table cells.

### Example: Extract Tables from Excel Spreadsheet

```csharp
using GroupDocs.Parser;
using GroupDocs.Parser.Options;
using GroupDocs.Parser.Data;
using GroupDocs.Parser.Templates;

// Excel file path
string excelPath = "sample.xlsx";

using (Parser parser = new Parser(excelPath))
{
    // Check if table extraction is supported for Excel
    if (!parser.Features.Tables)
    {
        Console.WriteLine("Table extraction is not supported for this Excel file.");
        return;
    }
    
    // Extract all tables from the Excel spreadsheet
    // Pass null to extract tables automatically
    IEnumerable<PageTableArea> tables = parser.GetTables(null);
    
    int tableIndex = 0;
    foreach (PageTableArea table in tables)
    {
        Console.WriteLine($"\n=== Table {++tableIndex} ===");
        Console.WriteLine($"Rows: {table.RowCount}, Columns: {table.ColumnCount}");
        
        // Iterate over rows
        for (int row = 0; row < table.RowCount; row++)
        {
            // Iterate over columns
            for (int column = 0; column < table.ColumnCount; column++)
            {
                // Get the table cell
                PageTableAreaCell cell = table[row, column];
                if (cell != null)
                {
                    // Print the cell text
                    Console.Write($"{cell.Text}\t");
                }
            }
            Console.WriteLine();
        }
    }
}
```

### Example: Extract Tables with Custom Layout

The following example shows how to extract tables from documents when you know the exact table structure (columns and rows positions):

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser(filePath))
{
    // Check if the document supports table extraction
    if (!parser.Features.Tables)
    {
        Console.WriteLine("Document isn't supports tables extraction.");
        return;
    }
    // Create the layout of tables
    TemplateTableLayout layout = new TemplateTableLayout(
        new double[] { 50, 95, 275, 415, 485, 545 },
        new double[] { 325, 340, 365, 395 });
    // Create the options for table extraction
    PageTableAreaOptions options = new PageTableAreaOptions(layout);
    // Extract tables from the document
    IEnumerable<PageTableArea> tables = parser.GetTables(options);
    // Iterate over tables
    foreach (PageTableArea t in tables)
    {
        // Iterate over rows
        for (int row = 0; row < t.RowCount; row++)
        {
            // Iterate over columns
            for (int column = 0; column < t.ColumnCount; column++)
            {
                // Get the table cell
                PageTableAreaCell cell = t[row, column];
                if (cell != null)
                {
                    // Print the table cell text
                    Console.Write(cell.Text);
                    Console.Write(" | ");
                }
            }
            Console.WriteLine();
        }
        Console.WriteLine();
    }
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

- [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)
- [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)

### Free online image extractor App

Along with full featured .NET library we provide simple, but powerfull free APPs.

You are welcome to extract images from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [GroupDocs Parser App](https://products.groupdocs.app/parser).