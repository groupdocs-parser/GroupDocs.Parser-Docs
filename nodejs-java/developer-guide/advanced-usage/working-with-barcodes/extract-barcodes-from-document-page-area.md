---
id: extract-barcodes-from-document-page-area
url: parser/nodejs-java/extract-barcodes-from-document-page-area
title: Extract barcodes from document page area
weight: 3
description: "Learn how to extract barcodes from a rectangular page area in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: extract barcodes from document page area, extract barcodes, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser provides the functionality to extract barcodes from a document page area by the `getBarcodes(PageAreaOptions)` method:

```javascript
parser.getBarcodes(options) // returns Iterable<PageBarcodeArea>
```

This method returns a Java `Iterable` collection of `PageBarcodeArea` objects:

| Member | Description |
| --- | --- |
| `getPage()` | The page that contains the barcode. |
| `getRectangle()` | The rectangular area on the page that contains the barcode. |
| `getValue()` | A string value that represents a value of the barcode page area. |
| `getCodeTypeName()` | A string value that represents a type name of the barcode. |

Here are the steps to extract barcodes from a page area:

- Instantiate the `Parser` object for the initial document;
- Check if the document supports barcodes extraction;
- Instantiate `PageAreaOptions` with the rectangular area;
- Call the `getBarcodes(PageAreaOptions)` method and obtain the collection of `PageBarcodeArea` objects;
- Iterate through the collection and get a barcode value.

The following example shows how to extract barcodes from the upper part of the pages (the area that contains QR codes in `Barcodes.pdf`):

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
    const options = new groupdocs.PageAreaOptions(
      new groupdocs.Rectangle(new groupdocs.Point(400, 50), new groupdocs.Size(200, 150)));
    // Extract barcodes from the page area
    const it = parser.getBarcodes(options).iterator();
    // Iterate over barcodes
    while (it.hasNext()) {
      const barcode = it.next();
      // Print the page index
      console.log('Page: ' + barcode.getPage().getIndex());
      // Print the barcode value
      console.log('Value: ' + barcode.getValue());
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

The output for `Barcodes.pdf`:

```text
Page: 0
Value: https://products.groupdocs.com/parser/net/
Page: 1
Value: https://products.groupdocs.com/parser/java/
```

{{< alert style="info" >}}Only barcodes that lie inside the rectangle are returned. Use `barcode.getRectangle()` from the [Extract barcodes from document](/parser/nodejs-java/extract-barcodes-from-document/) example to find the barcode coordinates in your document.{{< /alert >}}

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
