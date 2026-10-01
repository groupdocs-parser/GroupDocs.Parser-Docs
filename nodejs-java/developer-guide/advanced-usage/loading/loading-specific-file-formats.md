---
id: loading-specific-file-formats
url: parser/nodejs-java/loading-specific-file-formats
title: Loading specific file formats
weight: 3
description: "Learn how to specify the document format manually with LoadOptions in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: LoadOptions, FileFormat, markdown, specify file format, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

In some cases it's required to specify the document format manually to guarantee correct output produced by GroupDocs.Parser. The following are the cases when the document format must be specified manually:

* Markdown documents
* MHTML documents
* OTP documents (OpenDocument Presentation Template)
* Databases
* Emails from remote servers

Here are the steps to specify the document format for a Markdown document:

* Create an instance of the `LoadOptions` class and pass the document format to the `LoadOptions(FileFormat)` constructor;
* Create an instance of the `Parser` class with these options and call any method.

The following example shows how to specify the document format for a Markdown document loaded from a stream:

```javascript
const fs = require('fs');
const groupdocs = require('@groupdocs/groupdocs.parser');

groupdocs.readDataFromStream(fs.createReadStream('sample.md'), (stream) => {
  // Create an instance of Parser class for markdown document
  const parser = new groupdocs.Parser(stream, new groupdocs.LoadOptions(groupdocs.FileFormat.Markup));
  try {
    // Check if text extraction is supported
    if (!parser.getFeatures().isText()) {
      console.log("Text extraction isn't supported.");
    } else {
      const reader = parser.getText();
      try {
        // Print the document text
        console.log(reader.readToEnd());
      } finally {
        reader.close();
      }
    }
  } finally {
    parser.close();
  }
  process.exit(0);
});
```

A stream has no file name, so the file format can't be detected by the extension. `FileFormat.Markup` tells the parser to process the stream as a Markdown document.

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
