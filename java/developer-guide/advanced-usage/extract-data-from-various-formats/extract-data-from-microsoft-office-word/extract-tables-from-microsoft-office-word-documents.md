---
id: extract-tables-from-microsoft-office-word-documents
url: parser/java/extract-tables-from-microsoft-office-word-documents
title: Extract tables from Microsoft Office Word documents
weight: 5
description: "Learn how to easily extract table content from Word documents (.doc, .docx) using GroupDocs.Parser for Java."
keywords: extract tables, extract tables from Word, .doc, .docx, table parsing, Java
productName: GroupDocs.Parser for Java
hideChildren: False
---

This guide explains how to extract tables from Microsoft Office Word documents (`.doc`, `.docx`) using GroupDocs.Parser for Java.

## How table extraction works

GroupDocs.Parser converts a document's layout into a structured XML format. The [getStructure](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getStructure()) method returns this XML as an [*org.w3c.dom.Document*](https://docs.oracle.com/javase/7/docs/api/org/w3c/dom/Document.html?is-external=true) object. Tables are represented by `<table>` tags, rows by `<tr>` tags and cells by `<td>` tags. The extraction process involves reading this XML structure to identify and process table content. For more details, see [Extract text structure]({{< ref "parser/java/developer-guide/advanced-usage/working-with-text/extract-text-structure.md" >}}).

{{< alert style="warning" >}}[getStructure](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getStructure()) method returns null value if text structure extraction isn't supported for the document. For example, text structure extraction isn't supported for TXT files. Therefore, for TXT file [getStructure](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getStructure()) method returns null. If Microsoft Office Word document has no text, [getStructure](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getStructure()) method returns an empty org.w3c.dom.Document object.{{< /alert >}}

## Step-by-step guide

### 1. Create a Parser instance

Begin by initializing the [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) with your document path.

```java
try (Parser parser = new Parser("YourDocument.docx")) {
    // Extraction code goes here
}
```

### 2. Retrieve the document structure

Get the XML representation of the document's structure.

```java
Document document = parser.getStructure();
if (document == null) {
    System.out.println("Text structure extraction isn't supported for this document.");
    return;
}
// Process the XML structure
```

### 3. Find and process tables

Locate the table elements in the XML document.

```java
NodeList tables = document.getElementsByTagName("table");
for (int i = 0; i < tables.getLength(); i++) {
    System.out.println("Found a table:");
    processTable(tables.item(i));
}
```

### 4. Table processing method

This method handles the extraction of rows and cells from each table.

```java
private static void processTable(Node table) {
    int rowIndex = 0;
    NodeList rows = table.getChildNodes();
    for (int r = 0; r < rows.getLength(); r++) {
        Node row = rows.item(r);
        // Detect rows
        if (!"tr".equals(row.getNodeName())) {
            continue;
        }

        rowIndex++;
        System.out.println(String.format("[Row %d]", rowIndex));

        NodeList cells = row.getChildNodes();
        for (int c = 0; c < cells.getLength(); c++) {
            Node cell = cells.item(c);
            // Output the text of each cell
            if ("td".equals(cell.getNodeName())) {
                System.out.println("  " + cell.getTextContent());
            }
        }
    }
}
```

### Complete example

Here's a complete working example that extracts and displays tables from a Word document:

```java
import com.groupdocs.parser.Parser;
import org.w3c.dom.Document;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

public class WordTableExtractor {
    public static void extractTables(String filePath) {
        try (Parser parser = new Parser(filePath)) {
            Document document = parser.getStructure();
            if (document == null) {
                System.out.println("Text structure extraction isn't supported for this document.");
                return;
            }

            System.out.println("Analyzing: " + filePath);
            System.out.println("----------------------------------------");

            NodeList tables = document.getElementsByTagName("table");
            for (int i = 0; i < tables.getLength(); i++) {
                System.out.println();
                System.out.println(">>> TABLE FOUND");
                processTable(tables.item(i));
            }
        }
    }

    private static void processTable(Node table) {
        int rowIndex = 0;
        NodeList rows = table.getChildNodes();
        for (int r = 0; r < rows.getLength(); r++) {
            Node row = rows.item(r);
            if (!"tr".equals(row.getNodeName())) {
                continue;
            }

            rowIndex++;
            System.out.println(String.format("[Row %d]", rowIndex));

            NodeList cells = row.getChildNodes();
            for (int c = 0; c < cells.getLength(); c++) {
                Node cell = cells.item(c);
                if ("td".equals(cell.getNodeName())) {
                    System.out.println("  " + cell.getTextContent());
                }
            }
        }
    }
}
```

## Extract tables with the getTables method

Word documents also support the [getTables](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getTables()) method. It reads the tables of the document directly and returns a collection of [PageTableArea](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/PageTableArea) objects, so you can access cells by row and column index instead of walking the XML. The method returns null if table extraction isn't supported for the document; check it with the `isTables` method of [getFeatures](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getFeatures()). For more details, see [Extract tables from document]({{< ref "parser/java/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document.md" >}}).

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleDocx)) {
    // Check if the document supports table extraction
    if (!parser.getFeatures().isTables()) {
        System.out.println("Document doesn't support tables extraction.");
        return;
    }
    // Extract tables from the document
    Iterable<PageTableArea> tables = parser.getTables();
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

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
