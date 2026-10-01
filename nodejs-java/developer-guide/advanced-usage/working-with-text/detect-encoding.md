---
id: detect-encoding
url: parser/nodejs-java/detect-encoding
title: Detect encoding
weight: 8
description: "Detect the encoding of a plain text file with GroupDocs.Parser for Node.js via Java."
keywords: detect encoding of a plain text file, detect encoding, charset, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser provides the functionality to detect the encoding of a plain text file. The following encodings are supported:

*   UTF32 LE
*   UTF32 BE
*   UTF16 LE
*   UTF16 BE
*   UTF8
*   UTF7
*   ANSI

Encoding is detected by BOM or by the content of the file (if BOM isn't present).

Here are the steps to detect the encoding of the document:

*   Instantiate the `LoadOptions` object with the default ANSI encoding (a `java.nio.charset.Charset` object);
*   Instantiate the `Parser` object for the initial document;
*   Call the `getDocumentInfo` method and check that the result is a `com.groupdocs.parser.options.TextDocumentInfo` object (use `java.instanceOf`);
*   Call the `getCharset()` method.

The following example shows how to detect the encoding of the document:

```javascript
const java = require('java');
const groupdocs = require('@groupdocs/groupdocs.parser');
const Charset = java.import('java.nio.charset.Charset');

// Create an instance of LoadOptions class with the default ANSI encoding.
// This encoding is returned for ANSI text documents.
const loadOptions = new groupdocs.LoadOptions(groupdocs.FileFormat.WordProcessing, null, null, Charset.forName('US-ASCII'));
// Create an instance of Parser class
const parser = new groupdocs.Parser('utf8.txt', loadOptions);
try {
  // Get the document info
  const info = parser.getDocumentInfo();
  // Check if it's the document info of a plain text document
  if (!java.instanceOf(info, 'com.groupdocs.parser.options.TextDocumentInfo')) {
    console.log("Isn't a plain text document");
  } else {
    // Print the encoding
    console.log('Encoding: ' + info.getCharset().displayName());
  }
} finally {
  parser.close();
}
process.exit(0);
```

For the UTF-8 sample file the example prints `Encoding: UTF-8`.

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
