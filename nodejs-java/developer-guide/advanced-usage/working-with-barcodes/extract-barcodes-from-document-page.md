---
id: extract-barcodes-from-document-page
url: parser/nodejs-java/extract-barcodes-from-document-page
title: Extract barcodes from document page
weight: 2
description: "Learn how to extract barcodes from a specific document page in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: extract barcodes, extract barcodes from document page, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser provides the functionality to extract barcodes from a document page by the `getBarcodes(int)` method:

```javascript
parser.getBarcodes(pageIndex) // returns Iterable<PageBarcodeArea>
```

This method returns a Java `Iterable` collection of `PageBarcodeArea` objects:

| Member | Description |
| --- | --- |
| `getPage()` | The page that contains the barcode. |
| `getRectangle()` | The rectangular area on the page that contains the barcode. |
| `getValue()` | A string value that represents a value of the barcode page area. |
| `getCodeTypeName()` | A string value that represents a type name of the barcode. |

Here are the steps to extract barcodes from a document page:

- Instantiate the `Parser` object for the initial document;
- Check if the document supports barcodes extraction;
- Call the `getBarcodes(int)` method with the page index and obtain the collection of `PageBarcodeArea` objects;
- Iterate through the collection and get a barcode value.

The following example shows how to extract barcodes from the second document page:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('Barcodes.pdf');
try {
  // Check if the document supports barcodes extraction
  if (!parser.getFeatures().isBarcodes()) {
    console.log("Document doesn't support barcodes extraction.");
  } else {
    // Extract barcodes from the second document page
    const it = parser.getBarcodes(1).iterator();
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
Page: 1
Value: https://products.groupdocs.com/parser/java/
Page: 1
Value: 20211124
```

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
