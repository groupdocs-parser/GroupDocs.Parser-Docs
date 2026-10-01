---
id: extract-text-from-documents
url: parser/nodejs-java/extract-text-from-documents
title: Extract text from documents
weight: 5
description: "Extract text from PDF, emails, ebooks, Microsoft Office and LibreOffice documents in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: Extract text, EPUB, FB2, CHM, PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser allows to extract text from PDF, Emails, Ebooks, Microsoft Office formats: Word (DOC, DOCX), PowerPoint (PPT, PPTX), Excel (XLS, XLSX), LibreOffice formats and many others (see the full list in the [supported document formats](/parser/nodejs-java/supported-document-formats/) article).

GroupDocs.Parser's text extractor is easy to use and powerful at the same time (to resolve complex scenarios see the [advanced usage section](/parser/nodejs-java/advanced-usage/)).

This article demonstrates how to implement the simplest scenario - extract text from any supported format without additional settings.

## Extract text from documents

To extract text from documents simply call the `getText()` method of the `Parser` class:

```javascript
parser.getText(); // returns TextReader or null
```

This method returns an instance of the `TextReader` class with an extracted text, or `null` if text extraction isn't supported for the document.

`TextReader` has the following members:

| Member | Description |
| --- | --- |
| `readLine()` | Reads a line of characters from the text reader and returns the data as a string. |
| `readToEnd()` | Reads all characters from the current position to the end of the text reader and returns them as one string. |
| `close()` | Releases resources used by the reader. |

Here are the steps to extract a text from the document:

*   Instantiate the `Parser` object for the initial document;
*   Call the `getText()` method and obtain the `TextReader` object;
*   Check if *reader* isn't *null* (text extraction is supported for the document);
*   Read a text from *reader*;
*   Close the reader and the parser.

The following example shows how to extract a text from a document:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.pdf');
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

{{< alert style="info" >}}`process.exit(0)` is required at the end of a standalone script because the embedded JVM keeps the Node.js process alive.{{< /alert >}}

## More resources

### Advanced usage topics

To learn more about document data extraction features and get familiar how to extract text, images, forms and more, please refer to the [advanced usage section](/parser/nodejs-java/advanced-usage/).

### Free online document parser App

Along with the full-featured library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
