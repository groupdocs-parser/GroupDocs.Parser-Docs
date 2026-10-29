---
id: extract-tables-from-document
url: parser/java/extract-tables-from-document
title: Extract tables from document
weight: 4
description: "Learn how to extract tables from documents including Excel spreadsheets, Word documents, and PDFs using GroupDocs.Parser for Java. Complete guide with code examples for extracting tables from Excel in Java."
keywords: extract tables, extract tables from document, extract tables from Excel Java, Excel table extraction, extract tables from XLSX, Java table parser
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---

GroupDocs.Parser provides the functionality to extract tables from documents including **Excel spreadsheets (XLS, XLSX)**, Word documents, PDFs, and other supported formats by the [getTables(PageTableAreaOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getTables(com.groupdocs.parser.options.PageTableAreaOptions)) method:

```java
Iterable<PageTableArea> getTables(PageTableAreaOptions options);
```

This method returns a collection of [PageTableArea](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageTableArea) object:

| Member | Description |
| --- | --- |
| [getRectangle](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageArea#getRectangle()) | The rectangular area that bounds text area. |
| [getPage](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageArea#getPage()) | The page information (page index and page size) |
| [getRowCount](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageTableArea#getRowCount()) | The total number of the table rows. |
| [getColumnCount](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageTableArea#getColumnCount()) | The total number of the table columns. |
| [getCell(int, int)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageTableArea#getCell(int,%20int)) | The table cell by row and column indexes. |
| [getRowHeight(int)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageTableArea#getRowHeight(int)) | The the row height. |
| [getColumnWidth(int)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageTableArea#getColumnWidth(int)) | Returns the column width. |

[getTables(PageTableAreaOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getTables(com.groupdocs.parser.options.PageTableAreaOptions)) accepts [PageTableAreaOption](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/PageTableAreaOptions) object that contains [TemplateTableLayout](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.templates/TemplateTableLayout) object with table layout (see [this article]({{< ref "parser/java/developer-guide/advanced-usage/working-with-templates.md" >}}) for more details).

## Extract tables from Excel in Java

GroupDocs.Parser supports extracting structured table data from Excel spreadsheets. This is especially useful for processing data-rich Excel files programmatically.

For spreadsheets, [getTables()](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getTables()) (or [getTables(PageTableAreaOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getTables(com.groupdocs.parser.options.PageTableAreaOptions)) without a table layout) returns the Excel tables defined in the workbook (ranges formatted as a table) - one [PageTableArea](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageTableArea) per table. The page index of each table ([getPage](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageArea#getPage()).getIndex()) is the index of the worksheet that contains it. Use [getTables(int)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getTables(int)) to extract tables from a single worksheet. Plain cell ranges that are not formatted as tables are not returned; use [worksheet cells extraction]({{< ref "parser/java/developer-guide/advanced-usage/extract-data-from-various-formats/extract-data-from-microsoft-office-excel/extract-cells-from-microsoft-office-excel-spreadsheets.md" >}}) for them.

### Basic steps

Here are the basic steps to extract tables from an Excel spreadsheet:

1. Instantiate [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser) object for the spreadsheet;
2. Check if the document supports **table extraction** using [isTables](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/Features#isTables()) method;
3. Call [getTables()](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getTables()) method and obtain collection of [PageTableArea](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageTableArea) objects;
4. Iterate through the collection and access table cells.

### Example: extract tables from Excel spreadsheet

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleXlsx)) {
    // Check if table extraction is supported
    if (!parser.getFeatures().isTables()) {
        System.out.println("Table extraction is not supported for this Excel file.");
        return;
    }

    // Extract all tables from the Excel spreadsheet
    Iterable<PageTableArea> tables = parser.getTables();

    int tableIndex = 0;
    for (PageTableArea table : tables) {
        System.out.println(String.format("=== Table %d (worksheet %d) ===", ++tableIndex, table.getPage().getIndex()));
        System.out.println(String.format("Rows: %d, Columns: %d", table.getRowCount(), table.getColumnCount()));

        // Iterate over rows
        for (int row = 0; row < table.getRowCount(); row++) {
            // Iterate over columns
            for (int column = 0; column < table.getColumnCount(); column++) {
                // Get the table cell
                PageTableAreaCell cell = table.getCell(row, column);
                if (cell != null) {
                    // Print the cell text
                    System.out.print(cell.getText() + " | ");
                }
            }
            System.out.println();
        }
    }
}
```

## Extract tables with custom layout

When you know the exact table structure (columns and rows positions), pass it as [TemplateTableLayout](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.templates/TemplateTableLayout) in [PageTableAreaOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/PageTableAreaOptions). Here are the steps to extract tables from the whole document:

* Instantiate [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser) object for the initial document;
* Check if the document supports table extraction;
* Call [getTables(PageTableAreaOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getTables(com.groupdocs.parser.options.PageTableAreaOptions)) method and obtain collection of [PageTableArea](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageTableArea) objects;
* Iterate through the collection and print table cells.

The following example shows how to extract tables from the whole document:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleInvoicePagesPdf)) {
    // Check if the document supports table extraction
    if (!parser.getFeatures().isTables()) {
        System.out.println("Document isn't supports tables extraction.");
        return;
    }
    // Create the layout of tables
    TemplateTableLayout layout = new TemplateTableLayout(
            java.util.Arrays.asList(new Double[]{50.0, 95.0, 275.0, 415.0, 485.0, 545.0}),
            java.util.Arrays.asList(new Double[]{325.0, 340.0, 365.0, 395.0}));
    // Create the options for table extraction
    PageTableAreaOptions options = new PageTableAreaOptions(layout);
    // Extract tables from the document
    Iterable<PageTableArea> tables = parser.getTables(options);
    // Iterate over tables
    for (PageTableArea t : tables) {
        // Iterate over rows
        for (int row = 0; row < t.getRowCount(); row++) {
            // Iterate over columns
            for (int column = 0; column < t.getColumnCount(); column++) {
                // Get the table cell
                PageTableAreaCell cell = t.getCell(row, column);
                if (cell != null) {
                    // Print the table cell text
                    System.out.print(cell.getText());
                    System.out.print(" | ");
                }
            }
            System.out.println();
        }
        System.out.println();
    }
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

* [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)
* [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).