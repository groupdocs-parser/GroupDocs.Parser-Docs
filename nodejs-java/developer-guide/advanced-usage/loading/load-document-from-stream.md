---
id: load-document-from-stream
url: parser/nodejs-java/load-document-from-stream
title: Load document from stream
weight: 2
description: "Learn how to load a document from a Node.js readable stream or a Java input stream with GroupDocs.Parser for Node.js via Java."
keywords: load document from stream, readable stream, InputStream, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

There might be cases when the document is presented only as a stream (without a copy on the local disk). To avoid the overhead of saving documents to the disk, GroupDocs.Parser enables to extract data from streams directly.

The `Parser` constructor accepts a Java `java.io.InputStream`. The package provides the `readDataFromStream` helper that reads a Node.js readable stream (a file stream, an HTTP response, etc.) and passes its content to the callback as a Java input stream.

The following example shows how to load the document from a Node.js stream:

```javascript
const fs = require('fs');
const groupdocs = require('@groupdocs/groupdocs.parser');

// Read the Node.js stream into a Java input stream
groupdocs.readDataFromStream(fs.createReadStream('sample.pdf'), (stream) => {
  // Create an instance of Parser class with the stream
  const parser = new groupdocs.Parser(stream);
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
    stream.close();
  }
  process.exit(0);
});
```

`readDataFromStream` buffers the whole stream in memory. To read a file with a Java stream instead, create `java.io.FileInputStream` with the `java` module:

```javascript
const java = require('java');
const groupdocs = require('@groupdocs/groupdocs.parser');

const FileInputStream = java.import('java.io.FileInputStream');

// Create the stream
const stream = new FileInputStream('sample.pdf');
try {
  // Create an instance of Parser class with the stream
  const parser = new groupdocs.Parser(stream);
  try {
    // Extract a text into the reader
    const reader = parser.getText();
    // If text extraction isn't supported, a reader is null
    console.log(reader === null ? "Text extraction isn't supported" : reader.readToEnd());
    if (reader !== null) {
      reader.close();
    }
  } finally {
    parser.close();
  }
} finally {
  stream.close();
}
process.exit(0);
```

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
