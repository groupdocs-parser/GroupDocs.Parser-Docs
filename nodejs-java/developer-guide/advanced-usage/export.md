---
id: export-data
url: parser/nodejs-java/export-data
title: Export Data
weight: 106
description: "Learn how to export extracted data such as barcodes to XML files in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: xml export, export data, export barcodes, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser for Node.js via Java provides the functionality to export data to XML files. The `XmlExporter` class is used for this.

The following example shows how to extract barcodes from a document and export them to the XML file:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('Barcodes.pdf');
try {
  // Check if the document supports barcodes extraction
  if (!parser.getFeatures().isBarcodes()) {
    console.log("Document doesn't support barcodes extraction.");
  } else {
    // Create the options which are used for barcodes extraction
    const options = new groupdocs.BarcodeOptions(groupdocs.QualityMode.Low, groupdocs.QualityMode.Low, 'QR');
    // Extract barcodes from the document
    const barcodes = parser.getBarcodes(options);
    // Export data to "data.xml" file
    const exporter = new groupdocs.XmlExporter();
    exporter.exportBarcodes(barcodes, 'data.xml');
  }
} finally {
  parser.close();
}
process.exit(0);
```

For `Barcodes.pdf` the `data.xml` file contains the QR codes from both pages (formatted for readability):

```xml
<?xml version="1.0" ?>
<pages>
  <page index="0" width="1224.0" height="1584.0">
    <barcodeArea confidence="100" angle="0.0" codeTypeName="QR" left="445.0" top="64.5" right="550.5" bottom="169.5">https://products.groupdocs.com/parser/net/</barcodeArea>
  </page>
  <page index="1" width="1224.0" height="1584.0">
    <barcodeArea confidence="100" angle="0.0" codeTypeName="QR" left="446.0" top="65.0" right="552.0" bottom="170.0">https://products.groupdocs.com/parser/java/</barcodeArea>
  </page>
</pages>
```

`XmlExporter` also has `exportMetadata`, `exportDocumentInfo`, `exportTextAreas`, `exportTables` and `exportDocumentData` methods.

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
