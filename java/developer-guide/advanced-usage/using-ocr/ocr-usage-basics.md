---
id: ocr-usage-basics
url: parser/java/ocr-usage-basics
title: OCR Usage Basics
weight: 1
description:  "This article explains how to use OCR."
keywords: OCR, extract text from images
productName: GroupDocs.Parser for Java
hideChildren: False
---
GroupDocs.Parser doesn't contain OCR functionality as a part of its distributable. Instead of it API for integrating any paid or free OCR solution is provided. See [this article]({{< ref "use-ocr-connector" >}}) for details how to integrate OCR soluton to GroupDocs.Parser. 

To use OCR functionality, Parser object must be properly initialized:

* Instantiate [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings) object with the instance of class that implements OCR functionality;
* Instantiate [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/) object with [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings) object.

The following example shows how to create an instance of [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/) class with Aspose.OCR on-premise API connector:

```Java
// Create an instance of ParserSettings class with OCR Connector
ParserSettings settings = new ParserSettings(new AsposeOcrOnPremise());
// Create an instance of Parser class with settings
Parser parser = new Parser(Constants.SampleScan, settings);
```

[getText](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#getText-com.groupdocs.parser.options.TextOptions-) and [getTextAreas](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#getTextAreas-com.groupdocs.parser.options.PageTextAreaOptions-) methods from [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/) class are used to recognize a text from images.

To extract a text from image files or non-text PDF documents GetText method is used:

* Instantiate [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings) object with the instance of class that implements OCR functionality;
* Instantiate [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/) object with [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings) object;
* Instantiate [TextOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/textoptions/) object with useOcr = true;
* Call [getText](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#getText-com.groupdocs.parser.options.TextOptions-)(TextOptions) method with TextOptions parameter and obtain TextReader object;
* Check if the reader isn’t null (text extraction is supported for the document);
* Read a text from the reader.

The following example shows how to extract a text from the image file:

```Java
// Create an instance of ParserSettings class with OCR Connector
ParserSettings settings = new ParserSettings(new AsposeOcrOnPremise());
// Create an instance of Parser class with settings
try (Parser parser = new Parser(Constants.SampleScan, settings)) {
    // Create an instance of TextOptions to use OCR
    TextOptions options = new TextOptions(false, true);
    // Extract a text using OCR
    try (TextReader reader = parser.getText(options)) {
        // Print a text or 'not supported' message
        System.out.println(reader == null ? "Text extraction isn't supported" : reader.readToEnd());
    }
}
```

To extract text areas from image files or non-text PDF documents GetTextAreas method is used:

* Instantiate [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings) object with the instance of class that implements OCR functionality;
* Instantiate [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/) object with [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings) object;
* Instantiate [PageTextAreaOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/pagetextareaoptions) object with useOcr = true;
* Call [getTextAreas](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#getTextAreas-com.groupdocs.parser.options.PageTextAreaOptions-)(PageTextAreaOptions) method and obtain the collection of [PageTextArea](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/pagetextarea) objects;
* Check if the collection isn’t null (text areas extraction is supported for the document);
* Iterate through the collection and get rectangles and texts.

The following example shows how to extract text areas from the image file:

```Java
// Create an instance of ParserSettings class with OCR Connector
ParserSettings settings = new ParserSettings(new AsposeOcrOnPremise());
// Create an instance of Parser class with settings
try (Parser parser = new Parser(Constants.SampleScan, settings)) {
    // Create an instance of PageTextAreaOptions to use OCR
    PageTextAreaOptions options = new PageTextAreaOptions(true);
    // Extract text areas
    java.lang.Iterable<PageTextArea> areas = parser.getTextAreas(options);
    // Check if text areas extraction is supported
    if (areas == null) {
        System.out.println("Text areas extraction isn't supported");
        return;
    }
    // Iterate over text areas
    for (PageTextArea a : areas) {
        // Print a text, position and size for an each text area
        System.out.println(a.getText());
        System.out.println(String.format("\tPosition: (%d; %d)",
                a.getRectangle().getLeft(),
                a.getRectangle().getTop()));
        System.out.println(String.format("\tSize: (%d; %d)",
                a.getRectangle().getSize().getWidth(),
                a.getRectangle().getSize().getHeight()));
    }
}
```

[TextOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/textoptions/) and [PageTextAreaOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/pagetextareaoptions) classes have the property OcrOptions. OcrOptions class has the following members:

| Member | Description |
| --- | --- |
| Rectangle | Is used to pass a rectangular area to restrict the area of the text recognition. |
| Handler | An instance of [OcrEventHandler](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocreventhandler) class to handle any warnings which occur while the text recognition. |

The following sections describe how to use this property.

## How to restrict the area of the text recognition

To restrict an area of the image for the text recognition [OcrOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocroptions) class is used. Set [Rectangle](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocroptions/#getRectangle--) property to restrict the rectangular area for the text recognition.

The following example shows how to restrict the text recognition by the rectangular area:

```Java
// Create an instance of ParserSettings class with OCR Connector
ParserSettings settings = new ParserSettings(new AsposeOcrOnPremise());
// Create an instance of Parser class with settings
try (Parser parser = new Parser(Constants.SampleScan, settings)) {
    // Create an instance of OcrOptions to set a rectangle
    OcrOptions ocrOptions = new OcrOptions(new Rectangle(0, 0, 400, 200));
    // Create an instance of TextOptions to use OCR
    TextOptions options = new TextOptions(false, true, ocrOptions);
    // Extract a text using OCR
    try (TextReader reader = parser.getText(options)) {
        // Print a text or 'not supported' message
        System.out.println(reader == null ? "Text extraction isn't supported" : reader.readToEnd());
    }
}
```

## How to handle warnings

To restrict an area of the image for the text recognition [OcrOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocroptions) class is used. Set [Handler](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocroptions/#getHandler--) property to handle warning messages. [hasWarnings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocreventhandler/#hasWarnings--) property of [OcrEventHandler](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocreventhandler) class is used to indicate if any warnings occur. Use [Warnings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocreventhandler/#getWarnings--) to get all warnings or [getWarnings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocreventhandler/#getWarnings-int-) method for warnings for the page. The empty list returns if no warning occurs during the text recognition.

The following example shows how to handle warning messages:

```Java
// Create an instance of ParserSettings class with OCR Connector
ParserSettings settings = new ParserSettings(new AsposeOcrOnPremise());
// Create an instance of Parser class with settings
try (Parser parser = new Parser(Constants.SampleScan, settings)) {
    // Create an instance of OcrEventHandler to handle warnings
    OcrEventHandler handler = new OcrEventHandler();
    // Create an instance of OcrOptions to set a handler
    OcrOptions ocrOptions = new OcrOptions(null, handler);
    // Create an instance of TextOptions to use OCR
    TextOptions options = new TextOptions(false, true, ocrOptions);
    // Extract a text using OCR
    try (TextReader reader = parser.getText(options)) {
        // Print a text or 'not supported' message
        System.out.println(reader == null ? "Text extraction isn't supported" : reader.readToEnd());
    }
    if (handler.hasWarnings()) {
        System.out.println("The following warnings occur while text recognition:");
        for (String w : handler.getWarnings()) {
            System.out.println("\\t* " + w);
        }
    } else {
        System.out.println("Text recognition was performed without any warning.");
    }
}
```