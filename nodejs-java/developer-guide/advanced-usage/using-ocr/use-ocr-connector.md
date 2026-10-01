---
id: use-ocr-connector
url: parser/nodejs-java/use-ocr-connector
title: Use OCR Connector
weight: 2
description: "Learn how to create an OCR connector with Aspose.OCR for Java and plug it into GroupDocs.Parser for Node.js via Java."
keywords: OCR connector, OcrConnectorBase, Aspose.OCR, extract text from images, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

The `OcrConnectorBase` class provides the interface to integrate any OCR solution into GroupDocs.Parser. An OCR connector is a subclass of `OcrConnectorBase` which overrides the following methods:

| Method | Description |
| --- | --- |
| `recognizeText(InputStream imageStream, OcrOptions options)` | Extracts a text from the provided image stream. It is used when the `getText(TextOptions)` method of the `Parser` class is called with `useOcr = true`. |
| `recognizeTextAreas(InputStream imageStream, Size pageSize, OcrOptions options)` | Extracts text areas from the provided image stream. It is used when the `getTextAreas(PageTextAreaOptions)` method of the `Parser` class is called with `useOcr = true`. |
| `recognizeText(InputStream imageStream, int pageIndex, OcrOptions options)`, `recognizeTextAreas(InputStream imageStream, int pageIndex, Size pageSize, OcrOptions options)` | The same methods with the zero-based index of the document page. |

The base implementation of these methods returns `null`. A connector must override the methods for the required functionality. In version 26.9 the parser calls the overloads without the `pageIndex` parameter, so override them (or make them call the overloads with `pageIndex`, as shown below).

| Parameter | Description |
| --- | --- |
| `imageStream` | An image from which the text must be extracted. For a PDF document it's the rendered page. |
| `pageIndex` | A zero-based index of the page in the document (in the case when the image represents the document page). |
| `pageSize` | A size of the image (in the case when the image represents the document page - the size of the page). |
| `options` | Is used to define a rectangular area which restricts the area of the image and an `OcrEventHandler` object to handle warnings while the text recognition. |

## Why the connector is written in Java

`OcrConnectorBase` is a Java class, not an interface. The `java` module (node-java) can implement only Java interfaces in JavaScript (with `java.newProxy`); it can't create a subclass of a Java class. That is why an OCR connector is a small Java class compiled into a JAR and added to the classpath of the Node.js application. The OCR engine is also a Java library in this case.

## Aspose.OCR connector

The following connector uses Aspose.OCR for Java on-premise API. It requires these Maven artifacts (download the JAR files from Maven Central or your Maven repository):

* `com.aspose:aspose-ocr:22.11` (from the Aspose repository `https://releases.aspose.com/java/repo/`);
* `com.microsoft.onnxruntime:onnxruntime:1.11.0` (a dependency of Aspose.OCR).

```java
package com.example;

import com.groupdocs.parser.data.Page;
import com.groupdocs.parser.data.PageTextArea;
import com.groupdocs.parser.data.Point;
import com.groupdocs.parser.data.Rectangle;
import com.groupdocs.parser.data.Size;
import com.groupdocs.parser.options.OcrConnectorBase;
import com.groupdocs.parser.options.OcrOptions;

import javax.imageio.ImageIO;
import java.awt.image.BufferedImage;
import java.io.InputStream;
import java.util.ArrayList;

/**
 * OCR connector which uses Aspose.OCR for Java on-premise API.
 */
public class AsposeOcrOnPremise extends OcrConnectorBase {

    @Override
    public String recognizeText(InputStream imageStream, OcrOptions options) {
        return recognizeText(imageStream, 0, options);
    }

    @Override
    public String recognizeText(InputStream imageStream, int pageIndex, OcrOptions options) {
        try {
            com.aspose.ocr.RecognitionResult result = recognize(imageStream, pageIndex, options, false);
            // Return a recognized text
            return result.recognitionText;
        } catch (Exception ex) {
            return null;
        }
    }

    @Override
    public Iterable<PageTextArea> recognizeTextAreas(InputStream imageStream, Size pageSize, OcrOptions options) {
        return recognizeTextAreas(imageStream, 0, pageSize, options);
    }

    @Override
    public Iterable<PageTextArea> recognizeTextAreas(InputStream imageStream, int pageIndex, Size pageSize, OcrOptions options) {
        try {
            com.aspose.ocr.RecognitionResult result = recognize(imageStream, pageIndex, options, true);
            // Create a page object; for images the page index is always zero
            Page page = new Page(pageIndex, pageSize);
            // Combine rectangle and text collections to produce PageTextArea collection
            ArrayList<PageTextArea> areas = new ArrayList<>();
            for (int i = 0; i < result.recognitionAreasRectangles.size(); i++) {
                java.awt.Rectangle rect = result.recognitionAreasRectangles.get(i);
                String text = result.recognitionAreasText.get(i);
                areas.add(new PageTextArea(text, page, new Rectangle(
                        new Point(rect.getX(), rect.getY()), new Size(rect.getWidth(), rect.getHeight()))));
            }
            return areas;
        } catch (Exception ex) {
            return null;
        }
    }

    private static com.aspose.ocr.RecognitionResult recognize(
            InputStream imageStream, int pageIndex, OcrOptions options, boolean detectAreas) throws Exception {
        // Create an instance of Aspose OCR API
        com.aspose.ocr.AsposeOCR api = new com.aspose.ocr.AsposeOCR();
        // Read the image from the stream
        BufferedImage image = ImageIO.read(imageStream);
        // Create an instance of RecognitionSettings
        com.aspose.ocr.RecognitionSettings settings = new com.aspose.ocr.RecognitionSettings();
        settings.setDetectAreas(detectAreas);
        // Check if the rectangle is set
        if (options != null && options.getRectangle() != null) {
            Rectangle r = options.getRectangle();
            ArrayList<java.awt.Rectangle> areas = new ArrayList<>();
            areas.add(new java.awt.Rectangle(
                    (int) r.getLeft(), (int) r.getTop(),
                    (int) r.getSize().getWidth(), (int) r.getSize().getHeight()));
            // Set recognition areas
            settings.setRecognitionAreas(areas);
        }
        // Perform the text recognition
        com.aspose.ocr.RecognitionResult result = api.RecognizePage(image, settings);
        // Check if the handler is set
        if (options != null && options.getHandler() != null) {
            // Send all recognition warnings
            options.getHandler().onWarnings(pageIndex, result.warnings);
        }
        return result;
    }
}
```

## Build the connector

Save the class as `com/example/AsposeOcrOnPremise.java` and compile it with the JDK. The classpath contains the JAR of the `@groupdocs/groupdocs.parser` package and the Aspose.OCR JAR (on Linux and macOS use `:` instead of `;` as the classpath separator):

```bash
javac --release 8 -cp "node_modules/@groupdocs/groupdocs.parser/lib/groupdocs-parser-nodejs-26.9.jar;aspose-ocr-22.11.jar" -d out com/example/AsposeOcrOnPremise.java
jar cf ocr-connector.jar -C out .
```

## Use the connector

Add the connector JAR and the OCR engine JARs to `java.classpath` before the first call to the package, then pass an instance of the connector to the `ParserSettings` constructor:

```javascript
const path = require('path');
const java = require('java');
// Add the OCR connector and the OCR engine to the classpath before the JVM starts
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
  // Extract a text using OCR
  const reader = parser.getText(new groupdocs.TextOptions(false, true));
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

Without a license Aspose.OCR works in the evaluation mode: it recognizes only a part of the image and adds a trial message to the text. To set the Aspose.OCR license, call the static method `java.import('com.aspose.ocr.License').setLicense('Aspose.OCR.lic')` before the recognition.

See [OCR Usage Basics](/parser/nodejs-java/ocr-usage-basics/) for more examples.

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
