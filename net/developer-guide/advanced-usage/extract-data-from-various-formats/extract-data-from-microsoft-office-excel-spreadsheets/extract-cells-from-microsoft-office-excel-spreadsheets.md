---
id: extract-cells-from-microsoft-office-excel-spreadsheets
url: parser/net/extract-cells-from-microsoft-office-excel-spreadsheets
title: Extract cells from Microsoft Office Excel spreadsheets
weight: 5
description:  "This article explains that how to extract cells from Microsoft Office Excel (.xls, .xlsx) spreadsheets."
keywords: extract cells from Microsoft Office Excel, .xls, .xlsx
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---
GroupDocs.Parser provides the functionality to extract cells from spreadsheets. The following methods are used for this purpose:

* [GetWorksheetInfo](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/getworksheetinfo#getworksheetinfo_1)() - returns the information about the worksheet
* [GetWorksheetInfo](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/getworksheetinfo#getworksheetinfo)(int) - returns the information about all worksheets in the spreadsheet
* [GetWorksheetCells](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/getworksheetcells#getworksheetcells)(int) - returns the collection of cells
* [GetWorksheetCells](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/getworksheetcells#getworksheetcells_1)(int, WorksheetOptions) - returns a collection of cells contained in set ranges.

Here are the steps to extract cells from Microsoft Office Excel spreadsheet:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial spreadsheet;
*   Call [GetWorksheetCells](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/getworksheetcells#getworksheetcells)(int) method and obtain the collection of []() objects;
*   Iterate through the collection of [WorksheetCell](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/worksheetcell) and get a cell text value.

The following example shows how to extract cells from Microsoft Office Excel spreadsheet.

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser(Constants.SampleXlsx))
{
    // Check if worksheet cells extraction is supported
    if (!parser.Features.Worksheet)
    {
        throw new NotSupportedException("Worksheet cells extraction isn't supported");
    }

    // Get the information about worksheets
    IEnumerable<WorksheetInfo> info = parser.GetWorksheetInfo();
   
    // Iterate over worksheet information
    foreach(WorksheetInfo i in info)
    {
        // Print the worksheet name
        Console.WriteLine(i.Name);
        Console.WriteLine();

        // Get the worksheet cells
        IEnumerable<WorksheetCell> cells = parser.GetWorksheetCells(i.Index);

        // Iterate over cells
        foreach (WorksheetCell c in cells)
        {
            // Print the cell information and text value
            Console.WriteLine($"Row: {c.RowIndex} Column: {c.ColumnIndex} RowSpan: {c.RowSpan} ColumnSpan: {c.ColumnSpan}");
            Console.WriteLine(c.Text);
            Console.WriteLine();
        }
    }
}
```

The following example shows how to extract cells from Microsoft Office Excel spreadsheet with customization:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser(Constants.SampleXlsx))
{
    // Check if worksheet cells extraction is supported
    if (!parser.Features.Worksheet)
    {
        throw new NotSupportedException("Worksheet cells extraction isn't supported");
    }

    // Get the information about the first worksheet
    WorksheetInfo info = parser.GetWorksheetInfo(0);

    // Print the worksheet name
    Console.WriteLine(info.Name);
    Console.WriteLine();

    // Create the range that represents the first two rows
    WorksheetRange range = new WorksheetRange(
        info.MinRowIndex,
        Math.Min(info.MinRowIndex + 1, info.MaxRowIndex),
        info.MinColumnIndex,
        info.MaxColumnIndex);

    // Get the worksheet cells from the first two rows
    IEnumerable<WorksheetCell> cells = parser.GetWorksheetCells(0, new WorksheetOptions(range));

    // Iterate over cells
    foreach (WorksheetCell c in cells)
    {
        // Print the cell information and text value
        Console.WriteLine($"Row: {c.RowIndex} Column: {c.ColumnIndex} RowSpan: {c.RowSpan} ColumnSpan: {c.ColumnSpan}");
        Console.WriteLine(c.Text);
        Console.WriteLine();
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