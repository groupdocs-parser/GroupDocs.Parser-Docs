---
id: extract-tables-from-microsoft-office-word-documents
url: parser/net/extract-tables-from-microsoft-office-word-documents
title: Extract Tables from Microsoft Office Word Documents
weight: 5
description: "Learn how to easily extract table content from Word documents (.doc, .docx) using GroupDocs.Parser for .NET."
keywords: extract tables, extract tables from Word, .doc, .docx, table parsing
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---

This guide explains how to extract tables from Microsoft Office Word documents (`.doc`, `.docx`) using GroupDocs.Parser for .NET.

## How Table Extraction Works

GroupDocs.Parser converts a document's layout into a structured XML format. Tables are represented by `<table>` tags within this XML. The extraction process involves reading this XML structure to identify and process table content.

{{< alert style="warning" >}}
[GetStructure](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getstructure) method returns *null* value if text structure extraction isn't supported for the document. For example, text structure extraction isn't supported for TXT files. Therefore, for TXT file [GetStructure](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getstructure) method returns *null*. If Microsoft Office Word document has no text, [GetStructure](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getstructure)[](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getmetadata)method returns an empty [XmlReader](https://docs.microsoft.com/en-us/dotnet/api/system.xml.xmlreader?view=netframework-2.0) object.
{{< /alert >}}

## Step-by-Step Guide

### 1. Create a Parser Instance
Begin by initializing the Parser with your document path.

```csharp
using (Parser parser = new Parser("YourDocument.docx"))
{
    // Extraction code goes here
}
```

### 2. Retrieve Document Structure
Get the XML representation of the document's structure.

```csharp
using (XmlReader reader = parser.GetStructure())
{
    if (reader == null)
    {
        Console.WriteLine("Text structure extraction isn't supported for this document.");
        return;
    }
    // Process the XML structure
}
``` 

### 3. Find and Process Tables
Iterate through the XML to locate and process table elements.

```csharp
while (reader.Read())
{
    // Look for table start elements
    if (reader.NodeType == XmlNodeType.Element && reader.Name == "table")
    {
        Console.WriteLine("Found a table:");
        ProcessTable(reader);
    }
}
```

### 4. Table Processing Method
This method handles the extraction of rows and cells from each table.

```csharp
private static void ProcessTable(XmlReader reader)
{
    StringBuilder cellValue = new StringBuilder();
    int rowIndex = 0;

    while (reader.Read())
    {
        // Exit when table ends
        if (reader.NodeType == XmlNodeType.EndElement && reader.Name == "table")
        {
            break;
        }

        // Detect new rows
        if (reader.NodeType == XmlNodeType.Element && reader.Name == "tr")
        {
            rowIndex++;
            Console.WriteLine($"[Row {rowIndex}]");
        }

        // Reset cell value at cell start
        if (reader.NodeType == XmlNodeType.Element && reader.Name == "td")
        {
            cellValue.Clear();
        }

        // Output cell value at cell end
        if (reader.NodeType == XmlNodeType.EndElement && reader.Name == "td")
        {
            Console.WriteLine($"  {cellValue}");
        }

        // Accumulate cell text content
        if (reader.NodeType == XmlNodeType.Text)
        {
            cellValue.Append(reader.Value);
        }
    }
}
```

### Complete Example
Here's a complete working example that extracts and displays tables from a Word document:

```csharp
using System;
using System.Text;
using System.Xml;
using GroupDocs.Parser;

public static class WordTableExtractor
{
    public static void ExtractTables(string filePath)
    {
        using (Parser parser = new Parser(filePath))
        {
            using (XmlReader reader = parser.GetStructure())
            {
                if (reader == null)
                {
                    Console.WriteLine("Text structure extraction isn't supported for this document.");
                    return;
                }

                Console.WriteLine($"Analyzing: {filePath}");
                Console.WriteLine("----------------------------------------");

                while (reader.Read())
                {
                    if (reader.NodeType == XmlNodeType.Element && reader.Name == "table")
                    {
                        Console.WriteLine("\n>>> TABLE FOUND");
                        ProcessTable(reader);
                    }
                }
            }
        }
    }

    private static void ProcessTable(XmlReader reader)
    {
        StringBuilder cellValue = new StringBuilder();
        int rowIndex = 0;

        while (reader.Read())
        {
            if (reader.NodeType == XmlNodeType.EndElement && reader.Name == "table")
            {
                break;
            }

            if (reader.NodeType == XmlNodeType.Element && reader.Name == "tr")
            {
                rowIndex++;
                Console.WriteLine($"[Row {rowIndex}]");
            }

            if (reader.NodeType == XmlNodeType.Element && reader.Name == "td")
            {
                cellValue.Clear();
            }

            if (reader.NodeType == XmlNodeType.EndElement && reader.Name == "td")
            {
                Console.WriteLine($"  {cellValue}");
            }

            if (reader.NodeType == XmlNodeType.Text)
            {
                cellValue.Append(reader.Value);
            }
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