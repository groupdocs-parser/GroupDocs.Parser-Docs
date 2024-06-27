---
id: export-data
url: parser/java/export-data
title: Export Data
weight: 106
description: "How to export data to XML files."
keywords: xml export
productName: GroupDocs.Parser for Java
hideChildren: False
---
GroupDocs.Parser for Java provides the functionality to export data to XML files. [XmlExporter](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.export/xmlexporter) class is used for this. 

The following example shows how to extract barcodes from a document and export them to the XML file:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SamplePdfWithBarcodes))
{
    // Check if the document supports barcodes extraction
    if (!parser.getFeatures().isBarcodes())
    {
        System.out.println("Document doesn't support barcodes extraction.");
        return;
    }
    // Create the options which are used for barcodes extraction
    BarcodeOptions options = new BarcodeOptions(QualityMode.Low, QualityMode.Low, "QR");
    // Extract barcodes from the document
    Iterable<PageBarcodeArea> barcodes = parser.getBarcodes(options);
    // Export data to "data.xml" file
    XmlExporter exporter = new XmlExporter();
    exporter.exportBarcodes(barcodes, "data.xml");
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