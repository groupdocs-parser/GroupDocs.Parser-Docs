---
id: use-ocr-connector
url: parser/net/use-ocr-connector
title: Use OCR Connector
weight: 2
description:  "This article explains how to integrate OCR solution to GroupDocs.Parser"
keywords: OCR, extract text from images
productName: GroupDocs.Parser for .NET
hideChildren: False
---
[OcrConnectorBase](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocrconnectorbase) class provides the interface to integrate any OCR solution to GroupDocs.Parser. This class has the following members:

| Member | Description |
| --- | --- |
| RecognizeText | Extracts a text from the provided image stream. It is used when [GetText](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/gettext#gettext_1) method from Parser class is called. |
| RecognizeTextAreas | Extracts text areas from the provided image stream. It is used when [GetTextAreas](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/gettextareas#gettextareas_1) method from Parser class is called. |

Class with OCR integration must implement at least one of these methods (depends on the required functionality).

## Extract a text from the image

[RegognizeText](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocrconnectorbase/recognizetext) method has the following signature: 

```C#
string RecognizeText(Stream imageStream, int pageIndex, OcrOptions options)
```

| Parameter | Description |
| --- | --- |
| imageStream | An image from which the text must be extracted. |
| pageIndex | A zero-based index of the page in the document (in the case when the image represents the document page). |
| options | Is used to define a rectangular area which restricts the area of the image and [OcrEventHandler](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocreventhandler) object to handle warnings while the text recognition. |

The following example shows how to implement text recognition by using Aspose.OCR on-premise API:

```C#
public override string RecognizeText(Stream imageStream, int pageIndex, OcrOptions options)
{
    // Create an instance of Aspose OCR API
    AsposeOcr api = new AsposeOcr();

    // Convert the image stream into the memory stream
    using (MemoryStream memoryStream = GetMemoryStream(imageStream))
    {
        // Create an instance of RecognitionSettings
        RecognitionSettings settings = new RecognitionSettings();

        // Check if the rectangle is set
        if (options != null && options.Rectangle != null)
        {
            List<Aspose.Drawing.Rectangle> areas = new List<Aspose.Drawing.Rectangle>();
            areas.Add(new Aspose.Drawing.Rectangle(
                (int)options.Rectangle.Left,
                (int)options.Rectangle.Top,
                (int)options.Rectangle.Size.Width,
                (int)options.Rectangle.Size.Height));

            // Set recognition areas
            settings.RecognitionAreas = areas;
        }

        // Perform the text recognition
        RecognitionResult result = api.RecognizeImage(memoryStream, settings);

        // Check if the handler is set
        if (options != null && options.Handler != null)
        {
            // Send all recognition warnings
            options.Handler.OnWarnings(pageIndex, result.Warnings);
        }

        // Return a recognized text
        return result.RecognitionText;
    }
}
```

## Extract text areas from the image

[RecognizeTextAreas](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocrconnectorbase/recognizetextareas) method has the following signature:

```C#
IList<PageTextArea> RecognizeTextAreas(Stream imageStream, int pageIndex, Size pageSize, OcrOptions options)
```

| Parameter | Description |
| --- | --- |
| imageStream | An image from which the text must be extracted. |
| pageIndex | A zero-based index of the page in the document (in the case when the image represents the document page). |
| pageSize | A size of the image (in the case when the image represents the document page - the size of the page). |
| options | Is used to define a rectangular area which restricts the area of the image and [OcrEventHandler](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocreventhandler) object to handle warnings while the text recognition. |

The following example shows how to implement text areas recognition by using Aspose.OCR on-premise API:

```C#
public override IList<PageTextArea> RecognizeTextAreas(Stream imageStream, int pageIndex, Data.Size pageSize, OcrOptions options)
{
    // Create an instance of Aspose OCR API
    AsposeOcr api = new AsposeOcr();

    // Convert the image stream into the memory stream
    using (MemoryStream memoryStream = GetMemoryStream(imageStream))
    {
        // Create recognition settings and set detect areas
        RecognitionSettings settings = new RecognitionSettings(detectAreas: true);

        // Check if the rectangle is set
        if (options != null && options.Rectangle != null)
        {
            List<Aspose.Drawing.Rectangle> areas = new List<Aspose.Drawing.Rectangle>();
            areas.Add(new Aspose.Drawing.Rectangle(
                (int)options.Rectangle.Left,
                (int)options.Rectangle.Top,
                (int)options.Rectangle.Size.Width,
                (int)options.Rectangle.Size.Height));

            // Set recognition areas
            settings.RecognitionAreas = areas;
        }

        // Perform the text recognition 
        RecognitionResult r = api.RecognizeImage(memoryStream, settings);

        // Check if the handler is set
        if (options != null && options.Handler != null)
        {
            // Send all recognition warnings
            options.Handler.OnWarnings(pageIndex, r.Warnings);
        }

        // Create a page object. The pageIndex parameter represents the page index of the document; for images it's always zero.
        Page page = new Page(pageIndex, pageSize);

        // Combibe rectangle and text collections to produce PageTextArea collection
        return r.RecognitionAreasRectangles
            .Zip(r.RecognitionAreasText, (rect, text) => new { Rect = rect, Text = text })
            .Select(x => new PageTextArea(x.Text, page, new Data.Rectangle(x.Rect.Left, x.Rect.Top, x.Rect.Right, x.Rect.Bottom)))
            .ToList();
    }
}
```