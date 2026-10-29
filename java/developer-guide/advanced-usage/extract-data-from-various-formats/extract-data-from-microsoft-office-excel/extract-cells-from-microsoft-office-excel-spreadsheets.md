---
id: extract-cells-from-microsoft-office-excel-spreadsheets
url: parser/java/extract-cells-from-microsoft-office-excel-spreadsheets
title: Extract cells from Microsoft Office Excel spreadsheets
weight: 5
description:  "This article explains how to extract cells from Microsoft Office Excel (.xls, .xlsx) spreadsheets with GroupDocs.Parser for Java."
keywords: extract cells from Microsoft Office Excel, .xls, .xlsx, Java
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---
GroupDocs.Parser provides the functionality to extract cells from spreadsheets. The following methods are used for this purpose:

* [getWorksheetInfo()](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getWorksheetInfo()) - returns the information about all worksheets in the spreadsheet;
* [getWorksheetInfo(int)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getWorksheetInfo(int)) - returns the information about the worksheet with the specified index;
* [getWorksheetCells(int)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getWorksheetCells(int)) - returns the collection of cells of the worksheet;
* [getWorksheetCells(int, WorksheetOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getWorksheetCells(int,%20com.groupdocs.parser.options.WorksheetOptions)) - returns the collection of cells contained in the ranges set by [WorksheetOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/WorksheetOptions).

To check whether the document supports this functionality, use the [isWorksheet](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/Features#isWorksheet()) method of the [Features](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/Features) class. If cells extraction isn't supported, [getWorksheetCells](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getWorksheetCells(int)) and [getWorksheetInfo(int)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getWorksheetInfo(int)) methods return *null*.

Here are the steps to extract cells from Microsoft Office Excel spreadsheet:

*   Instantiate [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser) object for the initial spreadsheet;
*   Call [getWorksheetInfo](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getWorksheetInfo()) method and obtain the collection of [WorksheetInfo](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/WorksheetInfo) objects;
*   Call [getWorksheetCells(int)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getWorksheetCells(int)) method with the worksheet index and obtain the collection of [WorksheetCell](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/WorksheetCell) objects;
*   Iterate through the collection of [WorksheetCell](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/WorksheetCell) objects and get a cell text value.

The following example shows how to extract cells from Microsoft Office Excel spreadsheet:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleXlsx)) {
    // Check if worksheet cells extraction is supported
    if (!parser.getFeatures().isWorksheet()) {
        System.out.println("Worksheet cells extraction isn't supported.");
        return;
    }

    // Get the information about worksheets
    Iterable<WorksheetInfo> info = parser.getWorksheetInfo();

    // Iterate over worksheet information
    for (WorksheetInfo i : info) {
        // Print the worksheet name
        System.out.println(i.getName());
        System.out.println();

        // Get the worksheet cells
        Iterable<WorksheetCell> cells = parser.getWorksheetCells(i.getIndex());

        // Iterate over cells
        for (WorksheetCell c : cells) {
            // Print the cell information and text value
            System.out.println(String.format("Row: %d Column: %d RowSpan: %d ColumnSpan: %d",
                    c.getRowIndex(), c.getColumnIndex(), c.getRowSpan(), c.getColumnSpan()));
            System.out.println(c.getText());
            System.out.println();
        }
    }
}
```

[WorksheetInfo](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/WorksheetInfo) contains the worksheet index and name and the bounds of the data on the worksheet ([getMinRowIndex](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/WorksheetInfo#getMinRowIndex()), [getMaxRowIndex](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/WorksheetInfo#getMaxRowIndex()), [getMinColumnIndex](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/WorksheetInfo#getMinColumnIndex()), [getMaxColumnIndex](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/WorksheetInfo#getMaxColumnIndex())). Use these values to build a [WorksheetRange](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/WorksheetRange) and pass it to [WorksheetOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/WorksheetOptions) to extract only the cells you need. The [WorksheetOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/WorksheetOptions) constructor accepts one or more ranges.

The following example shows how to extract cells from Microsoft Office Excel spreadsheet with customization:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleXlsx)) {
    // Check if worksheet cells extraction is supported
    if (!parser.getFeatures().isWorksheet()) {
        System.out.println("Worksheet cells extraction isn't supported.");
        return;
    }

    // Get the information about the first worksheet
    WorksheetInfo info = parser.getWorksheetInfo(0);

    // Print the worksheet name
    System.out.println(info.getName());
    System.out.println();

    // Create the range that represents the first two rows
    WorksheetRange range = new WorksheetRange(
            info.getMinRowIndex(),
            Math.min(info.getMinRowIndex() + 1, info.getMaxRowIndex()),
            info.getMinColumnIndex(),
            info.getMaxColumnIndex());

    // Get the worksheet cells from the first two rows
    Iterable<WorksheetCell> cells = parser.getWorksheetCells(0, new WorksheetOptions(range));

    // Iterate over cells
    for (WorksheetCell c : cells) {
        // Print the cell information and text value
        System.out.println(String.format("Row: %d Column: %d RowSpan: %d ColumnSpan: %d",
                c.getRowIndex(), c.getColumnIndex(), c.getRowSpan(), c.getColumnSpan()));
        System.out.println(c.getText());
        System.out.println();
    }
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
