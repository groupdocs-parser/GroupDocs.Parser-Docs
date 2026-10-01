---
id: extract-data-from-attachments-and-zip-archives
url: parser/nodejs-java/extract-data-from-attachments-and-zip-archives
title: Extract data from attachments and ZIP archives
weight: 9
description: "Extract text, images and other data from ZIP-archived documents, email and PDF attachments in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: Extract data, ZIP archive, attachments, PDF portfolio, email attachments, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
It is easy to extract data, text, images and use any GroupDocs.Parser feature for ZIP-archived documents. The same feature allows to get attachments from PDF documents and emails and extract data from them.

## Extract data from attachments and ZIP archives

To extract documents from ZIP files and get attachments from containers simply call the `getContainer()` method:

```javascript
parser.getContainer(); // returns a Java Iterable of ContainerItem objects or null
```

This method returns a collection of `ContainerItem` objects:

| Member | Description |
| --- | --- |
| `getName()` | The name of the item. |
| `getDirectory()` | The directory of the item. |
| `getFilePath()` | The full path of the item. |
| `getSize()` | The size of the item in bytes. |
| `getMetadata()` | The collection of item metadata. |
| `detectFileType(mode)` | Detects a file type of the container item (`mode` is a `FileTypeDetectionMode` value). |
| `openStream()` | Opens the stream of the item content. |
| `openParser()` | Creates the `Parser` object for the item content. |
| `openParser(loadOptions)` | Creates the `Parser` object for the item content with `LoadOptions`. |
| `openParser(loadOptions, parserSettings)` | Creates the `Parser` object for the item content with `LoadOptions` and `ParserSettings`. |

A container represents both container-only files (like ZIP archives, Outlook storage) and documents with attachments (like emails, PDF Portfolios).

Here are the steps to extract a text from ZIP entities:

*   Instantiate the `Parser` object for the initial document;
*   Call the `getContainer()` method and obtain the collection of container item objects;
*   Check if *collection* isn't *null* (container extraction is supported for the document);
*   Iterate through the collection and obtain the `Parser` object to extract a text.

If the item format isn't supported, `openParser()` throws an error. The original Java exception (`UnsupportedDocumentFormatException`) is available in the `cause` property of the error.

The following example shows how to extract a text from ZIP entities:

```javascript
const java = require('java');
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.zip');
try {
  // Extract attachments from the container
  const attachments = parser.getContainer();
  // Check if container extraction is supported
  if (attachments === null) {
    console.log("Container extraction isn't supported");
  } else {
    // Iterate over zip entities
    const it = attachments.iterator();
    while (it.hasNext()) {
      const item = it.next();
      // Print the file path
      console.log(item.getFilePath());
      try {
        // Create Parser object for the zip entity content
        const attachmentParser = item.openParser();
        try {
          // Extract a zip entity text
          const reader = attachmentParser.getText();
          if (reader === null) {
            console.log('No text');
          } else {
            try {
              console.log(reader.readToEnd());
            } finally {
              reader.close();
            }
          }
        } finally {
          attachmentParser.close();
        }
      } catch (err) {
        // The Java exception is available in err.cause
        if (err.cause && java.instanceOf(err.cause, 'com.groupdocs.parser.exceptions.UnsupportedDocumentFormatException')) {
          console.log("Isn't supported.");
        } else {
          throw err;
        }
      }
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
