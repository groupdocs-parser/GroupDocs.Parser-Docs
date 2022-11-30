---
id: use-ocr-connector
url: parser/java/use-ocr-connector
title: Use OCR Connector
weight: 2
description:  "This article explains how to integrate OCR solution to GroupDocs.Parser"
keywords: OCR, extract text from images
productName: GroupDocs.Parser for Java
hideChildren: False
---
[OcrConnectorBase](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocrconnectorbase) class provides the interface to integrate any OCR solution to GroupDocs.Parser. This class has the following members:

| Member | Description |
| --- | --- |
| RecognizeText | Extracts a text from the provided image stream. It is used when [getText](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#getText-com.groupdocs.parser.options.TextOptions-) method from Parser class is called. |
| RecognizeTextAreas | Extracts text areas from the provided image stream. It is used when [getTextAreas](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#getTextAreas-com.groupdocs.parser.options.PageTextAreaOptions-) method from Parser class is called. |

Class with OCR integration must implement at least one of these methods (depends on the required functionality).

## Extract a text from the image

[RegognizeText](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocrconnectorbase/#recognizeText-java.io.InputStream-int-com.groupdocs.parser.options.OcrOptions-) method has the following signature: 

```C#
String recognizeText(java.io.InputStream imageStream, int pageIndex, OcrOptions options)
```

| Parameter | Description |
| --- | --- |
| imageStream | An image from which the text must be extracted. |
| pageIndex | A zero-based index of the page in the document (in the case when the image represents the document page). |
| options | Is used to define a rectangular area which restricts the area of the image and [OcrEventHandler](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocreventhandler) object to handle warnings while the text recognition. |

The following example shows how to implement text recognition by using Aspose.OCR on-premise API:

```Java
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

[RecognizeTextAreas](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocrconnectorbase/#recognizeTextAreas-java.io.InputStream-int-com.groupdocs.parser.data.Size-com.groupdocs.parser.options.OcrOptions-) method has the following signature:

```C#
IList<PageTextArea> RecognizeTextAreas(Stream imageStream, int pageIndex, Size pageSize, OcrOptions options)
```

| Parameter | Description |
| --- | --- |
| imageStream | An image from which the text must be extracted. |
| pageIndex | A zero-based index of the page in the document (in the case when the image represents the document page). |
| pageSize | A size of the image (in the case when the image represents the document page - the size of the page). |
| options | Is used to define a rectangular area which restricts the area of the image and [OcrEventHandler](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocreventhandler) object to handle warnings while the text recognition. |

The following example shows how to implement text areas recognition by using Aspose.OCR on-premise API:

```Java
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