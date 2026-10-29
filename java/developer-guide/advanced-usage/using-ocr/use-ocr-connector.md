---
id: use-ocr-connector
url: parser/java/use-ocr-connector
title: Use OCR Connector
weight: 2
description:  "This article explains how to integrate OCR solution to GroupDocs.Parser"
keywords: OCR, extract text from images
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---
GroupDocs.Parser for Java doesn't include a built-in OCR engine. Text recognition from images and scanned PDFs is performed by an OCR connector: a class that extends [OcrConnectorBase](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocrconnectorbase) and wraps any paid or free OCR solution. To use it, pass the connector when creating the Parser class:

* Instantiate [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings) object with the instance of class that implements OCR functionality;
* Instantiate [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/) object with [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings) object.

The following example shows how to create an instance of [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/) class with the custom connector:

```Java
// Create an instance of ParserSettings class with the custom OCR connector
ParserSettings settings = new ParserSettings(new YourCustomOcrConnector());
// Create an instance of Parser class with the parser settings
try (Parser parser = new Parser(filePath, settings)) {
    // Extract data from the document
}
```

## OcrConnectorBase members

[OcrConnectorBase](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocrconnectorbase) class has the following members. All of them have default implementations, so override only the members you need:

```Java
public class OcrConnectorBase {
    public boolean isTextSupported();

    public boolean isTextAreasSupported();

    public String recognizeText(InputStream imageStream, OcrOptions options);

    public Iterable<PageTextArea> recognizeTextAreas(InputStream imageStream, Size pageSize, OcrOptions options);

    @Deprecated
    public String recognizeText(InputStream imageStream, int pageIndex, OcrOptions options);

    @Deprecated
    public Iterable<PageTextArea> recognizeTextAreas(InputStream imageStream, int pageIndex, Size pageSize, OcrOptions options);
}
```

| Member | Description |
| --- | --- |
| [isTextSupported](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocrconnectorbase/#isTextSupported--) | Indicates whether the connector can extract plain text. Returns *true* by default. |
| [isTextAreasSupported](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocrconnectorbase/#isTextAreasSupported--) | Indicates whether the connector can extract text areas. Returns *true* by default. |
| [recognizeText(InputStream, OcrOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocrconnectorbase/#recognizeText-java.io.InputStream-com.groupdocs.parser.options.OcrOptions-) | Extracts a text from the provided image stream. It is used when [getText](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#getText-com.groupdocs.parser.options.TextOptions-) method from Parser class performs OCR - for image files and for documents with `useOcr = true`. |
| [recognizeTextAreas(InputStream, Size, OcrOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocrconnectorbase/#recognizeTextAreas-java.io.InputStream-com.groupdocs.parser.data.Size-com.groupdocs.parser.options.OcrOptions-) | Extracts text areas from the provided image stream. It is used when [getTextAreas](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#getTextAreas-com.groupdocs.parser.options.PageTextAreaOptions-) method from Parser class is called for image files. |
| [recognizeTextAreas(InputStream, int, Size, OcrOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocrconnectorbase/#recognizeTextAreas-java.io.InputStream-int-com.groupdocs.parser.data.Size-com.groupdocs.parser.options.OcrOptions-) | Deprecated. Extracts text areas from the image of the document page. In version 26.10 it is still used when [getTextAreas](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#getTextAreas-com.groupdocs.parser.options.PageTextAreaOptions-) method is called with `useOcr = true` for PDF and other documents. |
| [recognizeText(InputStream, int, OcrOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocrconnectorbase/#recognizeText-java.io.InputStream-int-com.groupdocs.parser.options.OcrOptions-) | Deprecated. The parser doesn't call this overload; implement [recognizeText(InputStream, OcrOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocrconnectorbase/#recognizeText-java.io.InputStream-com.groupdocs.parser.options.OcrOptions-) instead. |

Parser separates text recognition into two independent workflows: plain text and text areas. The first one is used when the text is extracted from a document, for example, by [getText](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#getText-com.groupdocs.parser.options.TextOptions-) method. The second one is used by [getTextAreas](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#getTextAreas-com.groupdocs.parser.options.PageTextAreaOptions-) method. When creating a custom connector, implement only the workflow you need and return *false* from the `isTextSupported` or `isTextAreasSupported` method for the other one. If `isTextSupported` returns *false*, text extraction from images isn't supported and [getText](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#getText--) returns *null* for them; if `isTextAreasSupported` returns *false*, [getTextAreas](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#getTextAreas-com.groupdocs.parser.options.PageTextAreaOptions-) returns *null* when OCR is required.

{{< alert style="warning" >}}
To support text areas both for image files and for PDF documents, implement both `recognizeTextAreas` overloads (one of them can delegate to the other).
{{< /alert >}}

The OCR settings are represented by the [OcrOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocroptions) class and may include:

* [OcrEventHandler](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocreventhandler) (`getHandler`) for sending warnings that occur during the text recognition;
* A rectangular area of the image (`getRectangle`) from which text needs to be extracted; in this case, there's no need to extract text from the entire image;
* A flag for using spell checking (`getUseSpellChecker`), the recognition language (`getLanguage`) and page preview options (`getPagePreviewOptions`) - see [OCR Usage Basics]({{< ref "parser/java/developer-guide/advanced-usage/using-ocr/ocr-usage-basics.md" >}}) for when these values reach the connector.

`options` can be *null*, so check it before use.

## Extract a text from the image

[recognizeText](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocrconnectorbase/#recognizeText-java.io.InputStream-com.groupdocs.parser.options.OcrOptions-) method has the following signature: 

```Java
public String recognizeText(java.io.InputStream imageStream, OcrOptions options)
```

| Parameter | Description |
| --- | --- |
| imageStream | An image from which the text must be extracted (the image file itself or the image of the document page). |
| options | Is used to define a rectangular area which restricts the area of the image and [OcrEventHandler](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocreventhandler) object to handle warnings while the text recognition. |

The method returns the recognized text; *null* if text recognition isn't supported.

The following example shows how to implement text recognition by using Aspose.OCR on-premise API. The recognition is implemented in the deprecated overload with the page index (as in the GitHub examples), and the overload which is called by the parser delegates to it:

```Java
@Override
public String recognizeText(java.io.InputStream imageStream, OcrOptions options) {
    // This overload is called by the parser; the page index isn't passed to it
    return recognizeText(imageStream, 0, options);
}

@Override
public String recognizeText(java.io.InputStream imageStream, int pageIndex, OcrOptions options) {
    try {
        // Create an instance of Aspose OCR API
        com.aspose.ocr.AsposeOCR api = new com.aspose.ocr.AsposeOCR();
        // Convert the image stream into the memory stream
        java.awt.image.BufferedImage image = ImageIO.read(imageStream);
        // Create an instance of RecognitionSettings
        com.aspose.ocr.RecognitionSettings settings = new com.aspose.ocr.RecognitionSettings();
        // Check if the rectangle is set
        if (options != null && options.getRectangle() != null) {
            ArrayList<java.awt.Rectangle> areas = new ArrayList<>();
            areas.add(new java.awt.Rectangle(
                    (int) options.getRectangle().getLeft(),
                    (int) options.getRectangle().getTop(),
                    (int) options.getRectangle().getSize().getWidth(),
                    (int) options.getRectangle().getSize().getHeight()));
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
        // Return a recognized text
        return result.recognitionText;
    } catch (java.lang.Exception ex) {
        return null;
    }
}
```

## Extract text areas from the image

[recognizeTextAreas](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocrconnectorbase/#recognizeTextAreas-java.io.InputStream-int-com.groupdocs.parser.data.Size-com.groupdocs.parser.options.OcrOptions-) methods have the following signatures:

```Java
public Iterable<PageTextArea> recognizeTextAreas(java.io.InputStream imageStream, Size pageSize, OcrOptions options)

@Deprecated
public Iterable<PageTextArea> recognizeTextAreas(java.io.InputStream imageStream, int pageIndex, Size pageSize, OcrOptions options)
```

| Parameter | Description |
| --- | --- |
| imageStream | An image from which the text must be extracted. |
| pageIndex | A zero-based index of the page in the document (in the case when the image represents the document page). |
| pageSize | A size of the image (in the case when the image represents the document page - the size of the rendered page image). |
| options | Is used to define a rectangular area which restricts the area of the image and [OcrEventHandler](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocreventhandler) object to handle warnings while the text recognition. |

Both methods return a collection of [PageTextArea](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/pagetextarea) objects (*null* if text areas recognition isn't supported). Each object contains the page ([Page](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/page) created from the page index and `pageSize`), the text and the rectangular area where the text was found. Return the rectangles in the coordinates of the passed image: for PDF and other documents the parser converts them to page coordinates itself.

The following example shows how to implement text areas recognition by using Aspose.OCR on-premise API:

```Java
@Override
public java.lang.Iterable<PageTextArea> recognizeTextAreas(java.io.InputStream imageStream, Size pageSize, OcrOptions options) {
    // This overload is called by the parser for image files
    return recognizeTextAreas(imageStream, 0, pageSize, options);
}

@Override
public java.lang.Iterable<PageTextArea> recognizeTextAreas(java.io.InputStream imageStream, int pageIndex, Size pageSize, OcrOptions options) {
    try {
        // Create an instance of Aspose OCR API
        com.aspose.ocr.AsposeOCR api = new com.aspose.ocr.AsposeOCR();
        // Convert the image stream into the memory stream
        java.awt.image.BufferedImage image = ImageIO.read(imageStream);
        // Create an instance of RecognitionSettings
        com.aspose.ocr.RecognitionSettings settings = new com.aspose.ocr.RecognitionSettings();
        settings.setDetectAreas(true);
        // Check if the rectangle is set
        if (options != null && options.getRectangle() != null) {
            ArrayList<java.awt.Rectangle> areas = new ArrayList<>();
            areas.add(new java.awt.Rectangle(
                    (int) options.getRectangle().getLeft(),
                    (int) options.getRectangle().getTop(),
                    (int) options.getRectangle().getSize().getWidth(),
                    (int) options.getRectangle().getSize().getHeight()));
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
        // Create a page object. The pageIndex parameter represents the page index of the document; for images it's always zero.
        Page page = new Page(pageIndex, pageSize);
        // Combine rectangle and text collections to produce PageTextArea collection
        ArrayList<PageTextArea> areas = new ArrayList<>();
        for(int i=0; i <result.recognitionAreasRectangles.size(); i++) {
            java.awt.Rectangle rect = result.recognitionAreasRectangles.get(i);
            String text = result.recognitionText;
            areas.add(new PageTextArea(text, page, new Rectangle(
                    new Point(rect.getX(), rect.getY()), new Size(rect.getWidth(), rect.getHeight()))));
        }
        return areas;
    } catch (java.lang.Exception ex) {
        return null;
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
