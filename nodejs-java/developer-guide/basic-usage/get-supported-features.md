---
id: get-supported-features
url: parser/nodejs-java/get-supported-features
title: Get supported features
weight: 3
description: "Check which data extraction features are supported for a document in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: Features, supported features, Parser, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
The set of the supported features depends on the document format. GroupDocs.Parser provides the functionality to check if a feature is supported for the document. The `getFeatures()` method of the `Parser` class is used for this purpose.

The `Features` class has the following members:

| Member | Description |
| --- | --- |
| `isFeatureSupported(feature)` | Returns the value that indicates whether the **feature** is supported. |
| `isText()` | The value that indicates whether **text extraction** is supported. |
| `isTextPage()` | The value that indicates whether **text page** extraction is supported. |
| `isFormattedText()` | The value that indicates whether **formatted text** extraction is supported. |
| `isFormattedTextPage()` | The value that indicates whether **formatted text page** extraction is supported. |
| `isSearch()` | The value that indicates whether **text search** is supported. |
| `isHighlight()` | The value that indicates whether **highlight extraction** is supported. |
| `isStructure()` | The value that indicates whether **text structure extraction** is supported. |
| `isToc()` | The value that indicates whether **table of contents extraction** is supported. |
| `isContainer()` | The value that indicates whether **container extraction** is supported. |
| `isMetadata()` | The value that indicates whether **metadata extraction** is supported. |
| `isTextAreas()` | The value that indicates whether **text areas extraction** is supported. |
| `isImages()` | The value that indicates whether **images extraction** is supported. |
| `isParseByTemplate()` | The value that indicates whether **parsing by template** is supported. |
| `isParseForm()` | The value that indicates whether **form parsing** is supported. |

Here are the steps to check if a feature is supported:

*   Instantiate the `Parser` object for the initial document;
*   Call the corresponding method of the object returned by `getFeatures()` to check if the feature is supported.

The following example shows how to check if the text extraction feature is supported:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.zip');
try {
  // Check if text extraction is supported for the document
  if (!parser.getFeatures().isText()) {
    console.log("Text extraction isn't supported");
  } else {
    // Extract a text from the document
    const reader = parser.getText();
    try {
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

If the feature isn't supported, the method returns *null* instead of the value. So if checking of `Features` is omitted, the result is checked for *null*:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.zip');
try {
  // Extract a text into the reader
  const reader = parser.getText();
  // If text extraction isn't supported, a reader is null
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
  parser.close();
}

process.exit(0);
```

Both examples print "Text extraction isn't supported" because there is no text in a ZIP archive.

Some operations may consume significant time, so it's not optimal to call the method just to check whether the feature is supported. Use `getFeatures()` for this purpose.

## More resources

### Advanced usage topics

To learn more about document data extraction features and get familiar how to extract text, images, forms and more, please refer to the [advanced usage section](/parser/nodejs-java/advanced-usage/).

### Free online document parser App

Along with the full-featured library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
