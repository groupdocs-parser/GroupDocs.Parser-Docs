---
id: get-document-info
url: parser/nodejs-java/get-document-info
title: Get document info
weight: 2
description: "Get the file type, page count and size of a document in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: Document Info, file type, page count, Parser, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser provides the functionality to get the basic document info by the `getDocumentInfo()` method:

```javascript
parser.getDocumentInfo(); // returns IDocumentInfo
```

The `IDocumentInfo` interface has the following members:

| Member | Description |
| --- | --- |
| `getFileType()` | The document type. |
| `getPageCount()` | The total number of document pages. |
| `getRawPageCount()` | The total number of document raw pages. |
| `getSize()` | The size of the document in bytes. |

Here are the steps to get document info:

*   Instantiate the `Parser` object for the initial document;
*   Call the `getDocumentInfo()` method and obtain the object with the `IDocumentInfo` interface;
*   Call methods such as `getFileType()`, `getPageCount()` or `getSize()`.

The following example shows how to get document info:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.docx');
try {
  // Get the document info
  const info = parser.getDocumentInfo();
  // Print document information
  console.log(`FileType: ${info.getFileType().toString()}`);
  console.log(`PageCount: ${info.getPageCount()}`);
  console.log(`Size: ${info.getSize()}`);
} finally {
  parser.close();
}

process.exit(0);
```

The output looks like this:

```text
FileType: Microsoft Word Open XML Document (.docx)
PageCount: 2
Size: 15214
```

## More resources

### Advanced usage topics

To learn more about document data extraction features and get familiar how to extract text, images, forms and more, please refer to the [advanced usage section](/parser/nodejs-java/advanced-usage/).

### Free online document parser App

Along with the full-featured library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
