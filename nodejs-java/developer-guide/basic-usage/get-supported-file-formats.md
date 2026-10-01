---
id: get-supported-file-formats
url: parser/nodejs-java/get-supported-file-formats
title: Get supported file formats
weight: 1
description: "Get the list of file formats supported by GroupDocs.Parser for Node.js via Java programmatically."
keywords: supported formats, file types, FileType, Parser, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser allows to get the list of all the supported file formats by the `getSupportedFileTypes()` static method of the `FileType` class:

```javascript
groupdocs.FileType.getSupportedFileTypes(); // returns a Java Iterable of FileType objects
```

This method returns a collection of `FileType` items with the following members:

| Member | Description |
| --- | --- |
| `getFileFormat()` | File type name, e.g. "Microsoft Word Document". |
| `getExtension()` | Filename suffix (including the period "."), e.g. ".doc". |

`FileType` also contains static fields that represent all the supported file formats (for example, `groupdocs.FileType.PDF`).

Here are the steps to get all the supported file formats:

*   Call the `getSupportedFileTypes()` static method and obtain a collection of `FileType` objects;
*   Iterate through the collection and call `getFileFormat()` or `getExtension()` of `FileType`.

The following example shows how to print all the supported file types:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Get a collection of supported file formats
const supportedFileTypes = groupdocs.FileType.getSupportedFileTypes();
// Iterate over collection and print file format information
const it = supportedFileTypes.iterator();
while (it.hasNext()) {
  const fileType = it.next();
  console.log(`${fileType.getFileFormat()} (${fileType.getExtension()})`);
}

process.exit(0);
```

The beginning of the output looks like this:

```text
Microsoft Word Document (.doc)
Word Document Template (.dot)
Microsoft Word Open XML Document (.docx)
Word Open XML Macro-Enabled Document (.docm)
...
```

## More resources

### Advanced usage topics

To learn more about document data extraction features and get familiar how to extract text, images, forms and more, please refer to the [advanced usage section](/parser/nodejs-java/advanced-usage/).

### Free online document parser App

Along with the full-featured library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
