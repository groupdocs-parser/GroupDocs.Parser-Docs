---
id: load-document-from-local-disk
url: parser/nodejs-java/load-document-from-local-disk
title: Load document from local disk
weight: 1
description: "Learn how to load a document from a local disk by its file path in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: load document, local disk, file path, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser for Node.js via Java provides the functionality to extract data from documents on the local disk. Pass the file path to the `Parser` constructor.

The following example shows how to load the document from the local disk:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Set the file path
const filePath = 'sample.pdf';
// Create an instance of Parser class with the file path
const parser = new groupdocs.Parser(filePath);
try {
  // Extract a text into the reader
  const reader = parser.getText();
  // If text extraction isn't supported, a reader is null
  if (reader === null) {
    console.log("Text extraction isn't supported");
  } else {
    try {
      // Print a text from the document
      console.log(reader.readToEnd());
    } finally {
      reader.close();
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

A relative path is resolved against the current working directory of the Node.js process. Use `path.join(__dirname, 'sample.pdf')` to resolve the path against the script folder.

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
