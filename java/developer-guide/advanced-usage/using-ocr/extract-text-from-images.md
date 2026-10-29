---
id: extract-text-from-images
url: parser/java/extract-text-from-images
title: Extract a text from images and PDFs
weight: 3
description:  "This article explains how to extract text from images and PDFs with GroupDocs.Parser for Java and an OCR connector"
keywords: OCR, extract text from images, Java
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---
GroupDocs.Parser for Java provides the ability to extract text from image files and PDFs composed of images.

{{< alert style="info" >}}GroupDocs.Parser for Java doesn't contain a built-in OCR engine. Text recognition is performed by an OCR connector - a class derived from [OcrConnectorBase](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/OcrConnectorBase) that wraps any paid or free OCR solution. The [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser) object must be created with [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ParserSettings) that contain the connector instance. See [Use OCR Connector]({{< ref "parser/java/developer-guide/advanced-usage/using-ocr/use-ocr-connector.md" >}}) for how to write one. The examples below use `AsposeOcrOnPremise` - the connector to Aspose.OCR for Java from the [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java).{{< /alert >}}

The list of recognition languages depends on the OCR engine behind the connector. The [Language](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/Language) value set by [OcrOptions.setLanguage](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/OcrOptions#setLanguage(com.groupdocs.parser.options.Language)) (`Language.Eng` by default) is only a hint for the connector: the connector must read it from the [OcrOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/OcrOptions) it receives and pass it to the OCR engine. See [OCR Usage Basics]({{< ref "parser/java/developer-guide/advanced-usage/using-ocr/ocr-usage-basics.md" >}}) for which options reach the connector.

## Extract text from a scanned PDF

For PDF documents OCR must be requested explicitly: create [TextOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/TextOptions) with `useOcr = true`. Without it, only the text layer of the PDF is extracted. If the parser was created without an OCR connector, the OCR request fails with `UnsupportedOperationException`.

The following example shows how to extract text from a scanned PDF:

```java
// Create an instance of ParserSettings class with OCR Connector
ParserSettings settings = new ParserSettings(new AsposeOcrOnPremise());

// Create an instance of Parser class with settings
try (Parser parser = new Parser("scanned.pdf", settings)) {
    // Create an instance of TextOptions to use OCR
    TextOptions options = new TextOptions(false, true);

    // Extract a text using OCR
    try (TextReader reader = parser.getText(options)) {
        // Print a text or 'not supported' message
        System.out.println(reader == null ? "Text extraction isn't supported" : reader.readToEnd());
    }
}
```

## Extract text from an image

For image files the text is always recognized by the OCR connector, so [TextOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/TextOptions) can be omitted. Text extraction from images is supported only if the parser has a connector whose [isTextSupported](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/OcrConnectorBase#isTextSupported()) method returns *true*; otherwise [getText](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getText()) returns *null*.

```java
// Create an instance of ParserSettings class with OCR Connector
ParserSettings settings = new ParserSettings(new AsposeOcrOnPremise());

// Create an instance of Parser class with settings
try (Parser parser = new Parser("scanned.jpg", settings)) {
    // Extract a text using OCR
    try (TextReader reader = parser.getText()) {
        // Print a text or 'not supported' message
        System.out.println(reader == null ? "Text extraction isn't supported" : reader.readToEnd());
    }
}
```

Pass [TextOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/TextOptions) with [OcrOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/OcrOptions) to send the recognition language and other settings to the connector:

```java
// Create an instance of ParserSettings class with OCR Connector
ParserSettings settings = new ParserSettings(new AsposeOcrOnPremise());

// Create an instance of Parser class with settings
try (Parser parser = new Parser("scanned.jpg", settings)) {
    // Set OCR options
    OcrOptions ocrOptions = new OcrOptions();
    ocrOptions.setLanguage(Language.Chinese);

    // Create an instance of TextOptions to use OCR
    TextOptions options = new TextOptions(false, true, ocrOptions);

    // Extract a text using OCR
    try (TextReader reader = parser.getText(options)) {
        // Print a text or 'not supported' message
        System.out.println(reader == null ? "Text extraction isn't supported" : reader.readToEnd());
    }
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
