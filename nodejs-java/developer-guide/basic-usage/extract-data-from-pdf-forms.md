---
id: extract-data-from-pdf-forms
url: parser/nodejs-java/extract-data-from-pdf-forms
title: Extract data from PDF forms
weight: 10
description: "Extract field values from PDF forms in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: Extract data, PDF form, extract pdf form data, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser allows to parse form data from PDF documents.

## Extract data from PDF forms

To extract PDF form data call the `parseForm()` method:

```javascript
parser.parseForm(); // returns DocumentData or null
```

This method returns an instance of the `DocumentData` class with the extracted data, or `null` if form parsing isn't supported for the document.

Here are the steps to parse a form of the document:

*   Instantiate the `Parser` object for the initial document;
*   Call the `parseForm()` method and obtain the `DocumentData` object;
*   Check if *data* isn't *null* (parse form is supported for the document);
*   Iterate over field data to obtain form data.

A field value is returned as a page area. Text values are `PageTextArea` objects. Use `java.instanceOf(object, className)` from the `java` package to check the Java type of a value (the JavaScript `instanceof` operator doesn't work with Java objects).

The following example shows how to parse a form of the document:

```javascript
const java = require('java');
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('Forms.pdf');
try {
  // Extract data from PDF document
  const data = parser.parseForm();
  // Check if form extraction is supported
  if (data === null) {
    console.log("Form extraction isn't supported.");
  } else {
    // Iterate over extracted data
    for (let i = 0; i < data.getCount(); i++) {
      const field = data.get(i);
      const area = field.getPageArea();
      // Check if the field value is a text area
      const isTextArea = area !== null && java.instanceOf(area, 'com.groupdocs.parser.data.PageTextArea');
      console.log(`${field.getName()}: ${isTextArea ? area.getText() : 'Not a template field'}`);
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
