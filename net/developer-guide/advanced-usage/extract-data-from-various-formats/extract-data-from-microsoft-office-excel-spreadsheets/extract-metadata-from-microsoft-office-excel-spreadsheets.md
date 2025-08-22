---
id: extract-metadata-from-microsoft-office-excel-spreadsheets
url: parser/net/extract-metadata-from-microsoft-office-excel-spreadsheets
title: Extract Metadata from Excel Spreadsheets in C# .NET
weight: 2
description: "Learn how to extract metadata from Microsoft Excel spreadsheets (.xls, .xlsx) in C# using GroupDocs.Parser for .NET. Step-by-step guide with code example."
keywords: extract Excel metadata C#, Excel spreadsheet metadata .NET, get Excel file properties C#, read Excel document metadata, GroupDocs.Parser metadata extraction
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---

# Extract Metadata from Excel Spreadsheets in C# .NET

Microsoft Excel files often contain hidden **metadata** such as the author, company, creation date, last modification time, and more. With **GroupDocs.Parser for .NET**, you can easily extract this metadata from Excel spreadsheets (`.xls` and `.xlsx`) using the [GetMetadata](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getmetadata) method.  

This guide explains how to extract Excel file metadata programmatically in just a few steps.

---

## What Metadata Can Be Extracted?


The `GetMetadata` method allows you to access a wide range of Excel spreadsheet properties:

| Metadata Field | Description |
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
| **revision-number** | Revision number |
| **total-editing-time** | Total editing time (in minutes) |

---

{{< alert style="warning" >}}
[GetMetadata](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getmetadata) method returns *null* value if metadata extraction isn't supported for the document. For example, metadata extraction isn't supported for CSV files. Therefore, for CSV file [GetMetadata](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getmetadata) method returns *null*. If Microsoft Office Excel spreadsheet has no metadata, [GetMetadata](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getmetadata) method returns an empty collection.
{{< /alert >}}

## How to Extract Excel Metadata in C#

Follow these steps to extract metadata from an Excel file:

1. **Instantiate the Parser object** for the Excel spreadsheet.  
2. **Call the `GetMetadata` method** to retrieve all metadata items.  
3. **Iterate through the collection** and print metadata names and values.  

```csharp
// Load the Excel file
using (Parser parser = new Parser(filePath))
{
    // Extract metadata
    IEnumerable<MetadataItem> metadata = parser.GetMetadata();
 
    // Iterate through metadata items
    foreach (MetadataItem item in metadata)
    {
        Console.WriteLine($"{item.Name}: {item.Value}");
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