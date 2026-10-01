---
id: ocr-usage-basics
url: parser/nodejs-java/ocr-usage-basics
title: OCR Usage Basics
weight: 1
description: "Learn how to extract a text and text areas from images and scanned PDFs with an OCR connector in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: OCR, extract text from images, scanned PDF, TextOptions, PageTextAreaOptions, OcrOptions, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser doesn't contain OCR functionality as a part of its distributable. Instead, an API for integrating any paid or free OCR solution is provided. See [Use OCR Connector](/parser/nodejs-java/use-ocr-connector/) for details how to create an OCR connector.

The examples below use the `com.example.AsposeOcrOnPremise` connector from that article. It is a Java class which uses Aspose.OCR for Java, so the connector JAR and the Aspose.OCR JARs must be added to the classpath before the first call to the package:

```javascript
const path = require('path');
const java = require('java');
// Add the OCR connector and the OCR engine to the classpath before the JVM starts
java.classpath.push(path.join(__dirname, 'ocr-connector.jar'));
java.classpath.push(path.join(__dirname, 'aspose-ocr-22.11.jar'));
java.classpath.push(path.join(__dirname, 'onnxruntime-1.11.0.jar'));
const groupdocs = require('@groupdocs/groupdocs.parser');
```

To use OCR functionality, the `Parser` object must be properly initialized:

* Create an instance of the `ParserSettings` class with the instance of the OCR connector;
* Create an instance of the `Parser` class with the `ParserSettings` object.

```javascript
const AsposeOcrOnPremise = java.import('com.example.AsposeOcrOnPremise');

// Create an instance of ParserSettings class with OCR Connector
const settings = new groupdocs.ParserSettings(new AsposeOcrOnPremise());
// Create an instance of Parser class with settings
const parser = new groupdocs.Parser('SampleScan.jpg', settings);
```

The `getText(TextOptions)` and `getTextAreas(PageTextAreaOptions)` methods of the `Parser` class are used to recognize a text.

## Extract a text

To extract a text from non-text PDF documents the `getText` method is used:

* Create an instance of the `ParserSettings` class with the instance of the OCR connector;
* Create an instance of the `Parser` class with the `ParserSettings` object;
* Create an instance of the `TextOptions` class with `useOcr = true` (the second parameter);
* Call the `getText(TextOptions)` method and obtain the `TextReader` object;
* Check if the reader isn't `null` (text extraction is supported for the document);
* Read a text from the reader.

The following example shows how to extract a text from a scanned PDF document:

```javascript
const path = require('path');
const java = require('java');
java.classpath.push(path.join(__dirname, 'ocr-connector.jar'));
java.classpath.push(path.join(__dirname, 'aspose-ocr-22.11.jar'));
java.classpath.push(path.join(__dirname, 'onnxruntime-1.11.0.jar'));
const groupdocs = require('@groupdocs/groupdocs.parser');

const AsposeOcrOnPremise = java.import('com.example.AsposeOcrOnPremise');

// Create an instance of ParserSettings class with OCR Connector
const settings = new groupdocs.ParserSettings(new AsposeOcrOnPremise());
// Create an instance of Parser class with settings
const parser = new groupdocs.Parser('scan.pdf', settings);
try {
  // Create an instance of TextOptions to use OCR
  const options = new groupdocs.TextOptions(false, true);
  // Extract a text using OCR
  const reader = parser.getText(options);
  if (reader === null) {
    console.log("Text extraction isn't supported");
  } else {
    try {
      // Print a text
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

{{< alert style="warning" >}}
In version 26.9 reading the OCR text of an image file (for example, `SampleScan.jpg`) with `getText(TextOptions)` fails with `java.lang.IllegalArgumentException: pageIndex` after the image is recognized. The same happens in GroupDocs.Parser for Java 26.9. Scanned PDF documents are not affected. To recognize a text of an image file, use `getTextAreas` as shown below.
{{< /alert >}}

## Extract text areas

To extract text areas from image files or non-text PDF documents the `getTextAreas` method is used:

* Create an instance of the `ParserSettings` class with the instance of the OCR connector;
* Create an instance of the `Parser` class with the `ParserSettings` object;
* Create an instance of the `PageTextAreaOptions` class with `useOcr = true`;
* Call the `getTextAreas(PageTextAreaOptions)` method and obtain the collection of `PageTextArea` objects;
* Check if the collection isn't `null` (text areas extraction is supported for the document);
* Iterate through the collection and get rectangles and texts.

The following example shows how to extract text areas from the image file:

```javascript
const path = require('path');
const java = require('java');
java.classpath.push(path.join(__dirname, 'ocr-connector.jar'));
java.classpath.push(path.join(__dirname, 'aspose-ocr-22.11.jar'));
java.classpath.push(path.join(__dirname, 'onnxruntime-1.11.0.jar'));
const groupdocs = require('@groupdocs/groupdocs.parser');

const AsposeOcrOnPremise = java.import('com.example.AsposeOcrOnPremise');

// Create an instance of ParserSettings class with OCR Connector
const settings = new groupdocs.ParserSettings(new AsposeOcrOnPremise());
// Create an instance of Parser class with settings
const parser = new groupdocs.Parser('SampleScan.jpg', settings);
try {
  // Create an instance of PageTextAreaOptions to use OCR
  const options = new groupdocs.PageTextAreaOptions(true);
  // Extract text areas
  const areas = parser.getTextAreas(options);
  // Check if text areas extraction is supported
  if (areas === null) {
    console.log("Text areas extraction isn't supported");
  } else {
    // Iterate over text areas
    const it = areas.iterator();
    while (it.hasNext()) {
      const a = it.next();
      // Print a text, position and size for an each text area
      console.log(a.getText());
      console.log(`\tPosition: (${a.getRectangle().getLeft()}; ${a.getRectangle().getTop()})`);
      console.log(`\tSize: (${a.getRectangle().getSize().getWidth()}; ${a.getRectangle().getSize().getHeight()})`);
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

The beginning of the output for `SampleScan.jpg`:

```
Lorem

	Position: (30; 15)
	Size: (123; 39)
Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Maecenas porttitor congue massa. Fusce posuere,
magna sed pulvinar ultricies, purus lectus malesuada libero, sit amet commodo magna eros quis urna.

	Position: (24; 59)
	Size: (1234; 80)
```

## OCR options

The `TextOptions` and `PageTextAreaOptions` classes accept an `OcrOptions` object. The `OcrOptions` class has the following members:

| Member | Description |
| --- | --- |
| `getRectangle()`, `setRectangle(Rectangle)` | A rectangular area which restricts the area of the text recognition. |
| `getHandler()`, `setHandler(OcrEventHandler)` | An instance of the `OcrEventHandler` class to handle warnings which occur while the text recognition. |

The OCR connector receives these options and must apply them. The following sections describe how to use them.

### How to restrict the area of the text recognition

To restrict an area of the image for the text recognition, set the rectangle in the `OcrOptions` constructor.

The following example shows how to restrict the text recognition by the rectangular area:

```javascript
const path = require('path');
const java = require('java');
java.classpath.push(path.join(__dirname, 'ocr-connector.jar'));
java.classpath.push(path.join(__dirname, 'aspose-ocr-22.11.jar'));
java.classpath.push(path.join(__dirname, 'onnxruntime-1.11.0.jar'));
const groupdocs = require('@groupdocs/groupdocs.parser');

const AsposeOcrOnPremise = java.import('com.example.AsposeOcrOnPremise');

// Create an instance of ParserSettings class with OCR Connector
const settings = new groupdocs.ParserSettings(new AsposeOcrOnPremise());
// Create an instance of Parser class with settings
const parser = new groupdocs.Parser('scan.pdf', settings);
try {
  // Create an instance of OcrOptions to set a rectangle
  const ocrOptions = new groupdocs.OcrOptions(new groupdocs.Rectangle(0, 0, 400, 200));
  // Create an instance of TextOptions to use OCR
  const options = new groupdocs.TextOptions(false, true, ocrOptions);
  // Extract a text using OCR
  const reader = parser.getText(options);
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

### How to handle warnings

To handle warning messages, pass an `OcrEventHandler` object to the `OcrOptions` constructor. The `hasWarnings` method of the `OcrEventHandler` class indicates if any warnings occur. Use the `getWarnings()` method to get all warnings or the `getWarnings(int)` method to get warnings for the page. An empty list is returned if no warning occurs during the text recognition.

The following example shows how to handle warning messages:

```javascript
const path = require('path');
const java = require('java');
java.classpath.push(path.join(__dirname, 'ocr-connector.jar'));
java.classpath.push(path.join(__dirname, 'aspose-ocr-22.11.jar'));
java.classpath.push(path.join(__dirname, 'onnxruntime-1.11.0.jar'));
const groupdocs = require('@groupdocs/groupdocs.parser');

const AsposeOcrOnPremise = java.import('com.example.AsposeOcrOnPremise');

// Create an instance of ParserSettings class with OCR Connector
const settings = new groupdocs.ParserSettings(new AsposeOcrOnPremise());
// Create an instance of Parser class with settings
const parser = new groupdocs.Parser('scan.pdf', settings);
try {
  // Create an instance of OcrEventHandler to handle warnings
  const handler = new groupdocs.OcrEventHandler();
  // Create an instance of OcrOptions to set a handler
  const ocrOptions = new groupdocs.OcrOptions(null, handler);
  // Create an instance of TextOptions to use OCR
  const options = new groupdocs.TextOptions(false, true, ocrOptions);
  // Extract a text using OCR
  const reader = parser.getText(options);
  if (reader === null) {
    console.log("Text extraction isn't supported");
  } else {
    try {
      console.log(reader.readToEnd());
    } finally {
      reader.close();
    }
  }
  if (handler.hasWarnings()) {
    console.log('The following warnings occur while text recognition:');
    const it = handler.getWarnings().iterator();
    while (it.hasNext()) {
      console.log('\t* ' + it.next());
    }
  } else {
    console.log('Text recognition was performed without any warning.');
  }
} finally {
  parser.close();
}
process.exit(0);
```

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
