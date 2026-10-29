---
id: extract-metadata-from-microsoft-office-excel-spreadsheets
url: parser/java/extract-metadata-from-microsoft-office-excel-spreadsheets
title: Extract metadata from Microsoft Office Excel spreadsheets
weight: 2
description: "Learn how to extract metadata from Microsoft Excel spreadsheets (.xls, .xlsx) in Java using GroupDocs.Parser for Java. Step-by-step guide with code example."
keywords: extract Excel metadata Java, Excel spreadsheet metadata Java, get Excel file properties Java, read Excel document metadata, GroupDocs.Parser metadata extraction
productName: GroupDocs.Parser for Java
hideChildren: False
---

Microsoft Excel files often contain hidden **metadata** such as the author, company, creation date, last modification time, and more. With **GroupDocs.Parser for Java**, you can easily extract this metadata from Excel spreadsheets (`.xls` and `.xlsx`) using the [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method.

This guide explains how to extract Excel file metadata programmatically in just a few steps.

---

## What metadata can be extracted?

The `getMetadata` method allows you to access a wide range of Excel spreadsheet properties:

| Name | Description |
| --- | --- |
| **title** | Spreadsheet title |
| **subject** | Spreadsheet subject |
| **keywords** | Spreadsheet keywords |
| **comments** | User comments |
| **content-status** | Status of the spreadsheet |
| **category** | Spreadsheet category |
| **company** | Company name |
| **manager** | Manager name |
| **author** | Original author |
| **last-author** | Last modified by |
| **hyperlink-base** | Base URL for relative hyperlinks |
| **application** | Application used to create the file |
| **application-version** | Version of the application |
| **template** | Template name |
| **created-time** | Creation time |
| **last-saved-time** | Last saved timestamp |
| **last-printed-time** | Last printed timestamp |
| **total-editing-time** | Total editing time (in minutes) |

Properties that are empty in the spreadsheet are not included in the result.

---

{{< alert style="warning" >}}[getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method returns null value if metadata extraction isn't supported for the document. For example, metadata extraction isn't supported for CSV files. Therefore, for CSV file [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method returns null. If Microsoft Office Excel spreadsheet has no metadata, [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method returns an empty collection.{{< /alert >}}

## How to extract Excel metadata in Java

Follow these steps to extract metadata from an Excel file:

1. **Instantiate the [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object** for the Excel spreadsheet.
2. **Call the [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method** to retrieve all [MetadataItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/MetadataItem) objects.
3. **Iterate through the collection** and print metadata names and values.

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleXlsx)) {
    // Extract metadata from the spreadsheet
    Iterable<MetadataItem> metadata = parser.getMetadata();
    // Iterate over metadata items
    for (MetadataItem item : metadata) {
        // Print an item name and value
        System.out.println(String.format("%s: %s", item.getName(), item.getValue()));
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
