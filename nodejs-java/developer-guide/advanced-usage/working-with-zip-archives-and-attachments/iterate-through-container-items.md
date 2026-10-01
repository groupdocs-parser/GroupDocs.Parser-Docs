---
id: iterate-through-container-items
url: parser/nodejs-java/iterate-through-container-items
title: Iterate through container items
weight: 1
description: "Learn how to iterate through items of ZIP archives and attachments and open them with a parser in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: extract container items, iterate through container items, ZIP, attachments, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser provides the functionality to extract items from containers by the `getContainer` method. This method returns a Java `Iterable` of `ContainerItem` objects (or `null` if container extraction isn't supported for the document):

| Member | Description |
| --- | --- |
| `getName()` | The name of the item. |
| `getDirectory()` | The directory of the item. |
| `getFilePath()` | The full path of the item. |
| `getSize()` | The size of the item in bytes. |
| `getMetadata()` | The collection of item metadata. |
| `openStream()` | Opens the stream of the item content. |
| `openParser()` | Creates the `Parser` object for the item content. |
| `openParser(LoadOptions)` | Creates the `Parser` object for the item content with `LoadOptions`. |
| `openParser(LoadOptions, ParserSettings)` | Creates the `Parser` object for the item content with `LoadOptions` and `ParserSettings`. |
| `detectFileType(FileTypeDetectionMode)` | Detects the file type of the item. See [Detect file type of container item](/parser/nodejs-java/detect-file-type-of-container-item/). |

Here are the steps to extract a container from the document:

* Create an instance of the `Parser` class for the initial document;
* Call the `getContainer` method and obtain the collection of `ContainerItem` objects;
* Check if the collection isn't `null` (container extraction is supported for the document);
* Iterate through the collection and get container item names, sizes and obtain content.

The following example shows how to extract items from a ZIP archive:

```javascript
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
    // Iterate over attachments
    const it = attachments.iterator();
    while (it.hasNext()) {
      const item = it.next();
      // Print an item name and size
      console.log(`${item.getName()}: ${item.getSize()}`);
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

The output for `sample.zip`:

```
sample.docx: 23478
sample.pdf: 150567
images.pdf: 337143
th.jpg: 17261
1200px-RedPandaFullBody.JPG: 436672
```

The following example shows how to extract a text from each container item with the `openParser` method:

```javascript
const java = require('java');
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.zip');
try {
  // Extract attachments from the container
  const attachments = parser.getContainer();
  if (attachments === null) {
    console.log("Container extraction isn't supported");
  } else {
    const it = attachments.iterator();
    while (it.hasNext()) {
      const item = it.next();
      console.log(item.getFilePath());
      try {
        // Create Parser object for the item content
        const itemParser = item.openParser();
        try {
          // Extract a text from the item
          const reader = itemParser.getText();
          if (reader === null) {
            console.log("Text extraction isn't supported");
          } else {
            try {
              console.log(reader.readToEnd());
            } finally {
              reader.close();
            }
          }
        } finally {
          itemParser.close();
        }
      } catch (err) {
        if (err.cause && java.instanceOf(err.cause, 'com.groupdocs.parser.exceptions.UnsupportedDocumentFormatException')) {
          console.log("Document format isn't supported");
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

For JPG images of `sample.zip` this example prints "Text extraction isn't supported".

Container represents both container-only files (like ZIP archives, Outlook storages) and documents with attachments (like emails, PDF Portfolios).

In case of Outlook storage (OST/PST files) the container consists of email documents (MSG files).

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
