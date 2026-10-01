---
id: detect-file-type-of-container-item
url: parser/nodejs-java/detect-file-type-of-container-item
title: Detect file type of container item
weight: 2
description: "Learn how to detect the file type of ZIP archive items and attachments in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: detect file type, detect file type of container item, FileTypeDetectionMode, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser provides the functionality to detect a file type of container items by the `detectFileType(FileTypeDetectionMode)` method of the `ContainerItem` class.

The `FileTypeDetectionMode` parameter provides the ability to control file type detection:

* **Default**. The file type is detected by the file extension; if the file extension isn't recognized, the file type is detected by the file content.
* **Extension**. The file type is detected only by the file extension.
* **Content**. The file type is detected only by the file content.

Here are the steps to detect a file type of container items:

* Create an instance of the `Parser` class for the initial document;
* Call the `getContainer` method and obtain the collection of `ContainerItem` objects;
* Check if the collection isn't `null` (container extraction is supported for the document);
* Iterate through the collection and call the `detectFileType(FileTypeDetectionMode)` method.

The following example shows how to detect a file type of container items:

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
      // Detect the file type
      const fileType = item.detectFileType(groupdocs.FileTypeDetectionMode.Default);
      // Print the name and file type
      console.log(`${item.getName()}: ${fileType}`);
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

The output for `sample.zip`:

```
sample.docx: Microsoft Word Open XML Document (.docx)
sample.pdf: Portable Document Format File (.pdf)
images.pdf: Portable Document Format File (.pdf)
th.jpg: JPEG Image (.jpg)
1200px-RedPandaFullBody.JPG: JPEG Image (.jpg)
```

The returned `FileType` object also has `getExtension()` and `getFileFormat()` methods.

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
