---
id: extract-metadata-from-documents
url: parser/nodejs-java/extract-metadata-from-documents
title: Extract metadata from documents
weight: 7
description: "Extract metadata from PDF, emails, ebooks, Microsoft Office and LibreOffice documents in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: Extract metadata, extract basic metadata, document properties, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser allows to extract basic metadata from documents of various formats: PDF, Emails, Ebooks, Microsoft Office: Word (DOC, DOCX), PowerPoint (PPT, PPTX), Excel (XLS, XLSX), LibreOffice formats and many others (see the full list in the [supported document formats](/parser/nodejs-java/supported-document-formats/) article).

## Extract metadata from documents

To extract metadata from documents simply call the `getMetadata()` method:

```javascript
parser.getMetadata(); // returns a Java Iterable of MetadataItem objects or null
```

This method returns a collection of `MetadataItem` objects with the following members:

| Member | Description |
| --- | --- |
| `getName()` | The name of the metadata item. |
| `getValue()` | The value of the metadata item. |

Here are the steps to extract metadata from the document:

*   Instantiate the `Parser` object for the initial document;
*   Call the `getMetadata()` method and obtain the collection of document metadata objects;
*   Check if *collection* isn't *null* (metadata extraction is supported for the document);
*   Iterate through the collection and get metadata names and values.

The following example shows how to extract metadata from a document:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.docx');
try {
  // Extract metadata from the document
  const metadata = parser.getMetadata();
  // Check if metadata extraction is supported
  if (metadata === null) {
    console.log("Metadata extraction isn't supported");
  } else {
    // Iterate over metadata items
    const it = metadata.iterator();
    while (it.hasNext()) {
      const item = it.next();
      // Print an item name and value
      console.log(`${item.getName()}: ${item.getValue()}`);
    }
  }
} finally {
  parser.close();
}

process.exit(0);
```

## More resources

### Advanced usage topics

To learn more about document data extraction features and get familiar how to extract text, images, forms and more, please refer to the [advanced usage section](/parser/nodejs-java/advanced-usage/).

### Free online document parser App

Along with the full-featured library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
