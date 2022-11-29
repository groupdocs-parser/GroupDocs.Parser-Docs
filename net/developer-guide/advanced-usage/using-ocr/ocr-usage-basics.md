---
id: ocr-usage-basics
url: parser/net/ocr-usage-basics
title: OCR Usage Basics
weight: 1
description:  "This article explains how to use OCR."
keywords: OCR, extract text from images
productName: GroupDocs.Parser for .NET
hideChildren: False
---
GroupDocs.Parser doesn't contain OCR functionality as a part of its distributable. Instead of it API for integrating any paid or free OCR solution is provided. See [this article]({{< ref "use-ocr-connector" >}}) for details how to integrate OCR soluton to GroupDocs.Parser. 

To use OCR functionality, Parser object must be properly initialized:

* Instantiate [ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings) object with the instance of class that implements OCR functionality;
* Instantiate [Parser](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/) object with [ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings) object.

The following example shows how to create an instance of [Parser](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/) class with Aspose.OCR on-premise API connector:

```C#
// Create an instance of ParserSettings class with the implementation of Aspose.OCR on-premise API connector
ParserSettings settings = new ParserSettings(new AsposeOCR());
// Create an instance of Parser class with the parser settings
Parser parser = new Parser(fileName, settins);
```

[GetText](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/gettext#gettext_1) and [GetTextAreas](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/gettextareas#gettextareas_1) methods from [Parser](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/) class are used to recognize a text from images.

To extract a text from image files or non-text PDF documents GetText method is used:

* Instantiate [ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings) object with the instance of class that implements OCR functionality;
* Instantiate [Parser](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/) object with [ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings) object;
* Instantiate [TextOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/textoptions/) object with useOcr = true;
* Call [GetText](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/gettext#gettext_1)(TextOptions) method with TextOptions parameter and obtain TextReader object;
* Check if the reader isn’t null (text extraction is supported for the document);
* Read a text from the reader.

The following example shows how to extract a text from the image file:

```C#
// Create an instance of ParserSettings class with OCR Connector
ParserSettings settings = new ParserSettings(new AsposeOcrOnPremise());

// Create an instance of Parser class with settings
using (Parser parser = new Parser(Constants.SampleScan, settings))
{
    // Create an instance of TextOptions to use OCR
    TextOptions options = new TextOptions(false, true);
    // Extract a text using OCR
    using(TextReader reader = parser.GetText(options))
    {
        // Print a text or 'not supported' message
        Console.WriteLine(reader == null ? "Text extraction isn't supported" : reader.ReadToEnd());
    }
}
```

To extract text areas from image files or non-text PDF documents GetTextAreas method is used:

* Instantiate [ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings) object with the instance of class that implements OCR functionality;
* Instantiate [Parser](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/) object with [ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings) object;
* Instantiate [PageTextAreaOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/pagetextareaoptions) object with useOcr = true;
* Call [GetTextAreas](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/gettextareas/#gettextareas_1)(PageTextAreaOptions) method and obtain the collection of [PageTextArea](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagetextarea) objects;
* Check if the collection isn’t null (text areas extraction is supported for the document);
* Iterate through the collection and get rectangles and texts.

The following example shows how to extract text areas from the image file:

```C#
// Create an instance of ParserSettings class with OCR Connector
ParserSettings settings = new ParserSettings(new AsposeOcrOnPremise());

// Create an instance of Parser class with settings
using (Parser parser = new Parser(Constants.SampleScan, settings))
{
    // Create an instance of PageTextAreaOptions to use OCR
    PageTextAreaOptions options = new PageTextAreaOptions(true);
  
    // Extract text areas
    IEnumerable<PageTextArea> areas = parser.GetTextAreas(options);
    
    // Check if text areas extraction is supported
    if (areas == null)
    {
        Console.WriteLine("Text areas extraction isn't supported");
        return;
    }

    // Iterate over text areas
    foreach (PageTextArea a in areas)
    {
        // Print a text, position and size for each text area
        Console.WriteLine(a.Text);
        Console.WriteLine("\tPosition: ({0}; {1})", a.Rectangle.Left, a.Rectangle.Top);
        Console.WriteLine("\tSize: ({0}; {1})", a.Rectangle.Size.Width, a.Rectangle.Size.Height);
    }
}
```

[TextOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/textoptions/) and [PageTextAreaOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/pagetextareaoptions) classes have the property OcrOptions. OcrOptions class has the following members:

| Member | Description |
| --- | --- |
| Rectangle | Is used to pass a rectangular area to restrict the area of the text recognition. |
| Handler | An instance of [OcrEventHandler](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocreventhandler) class to handle any warnings which occur while the text recognition. |

The following sections describe how to use this property.

## How to restrict the area of the text recognition

To restrict an area of the image for the text recognition [OcrOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocroptions) class is used. Set [Rectangle](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocroptions/rectangle) property to restrict the rectangular area for the text recognition.

The following example shows how to restrict the text recognition by the rectangular area:

```C#
// Create an instance of ParserSettings class with OCR Connector
ParserSettings settings = new ParserSettings(new AsposeOcrOnPremise());

// Create an instance of Parser class with settings
using (Parser parser = new Parser(Constants.SampleScan, settings))
{
    // Create an instance of OcrOptions to set a rectangle
    OcrOptions ocrOptions = new OcrOptions(new Data.Rectangle(0, 0, 400, 200));

    // Create an instance of TextOptions to use OCR
    TextOptions options = new TextOptions(false, true, ocrOptions);
    // Extract a text using OCR
    using (TextReader reader = parser.GetText(options))
    {
        // Print a text or 'not supported' message
        Console.WriteLine(reader == null ? "Text extraction isn't supported" : reader.ReadToEnd());
    }
}
```

## How to handle warnings

To restrict an area of the image for the text recognition [OcrOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocroptions) class is used. Set [Handler](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocroptions/handler) property to handle warning messages. [HasWarnings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocreventhandler/haswarnings) property of [OcrEventHandler](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocreventhandler) class is used to indicate if any warnings occur. Use [Warnings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocreventhandler/warnings) to get all warnings or [GetWarnings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocreventhandler/getwarnings) method for warnings for the page. the empty list returns if no warning occurs during the text recognition.

The following example shows how to handle warning messages:

```C#
// Create an instance of ParserSettings class with OCR Connector
ParserSettings settings = new ParserSettings(new AsposeOcrOnPremise());

// Create an instance of Parser class with settings
using (Parser parser = new Parser(Constants.SampleScan, settings))
{
    // Create an instance of OcrEventHandler to handle warnings
    OcrEventHandler handler = new OcrEventHandler();

    // Create an instance of OcrOptions to set a handler
    OcrOptions ocrOptions = new OcrOptions(handler);

    // Create an instance of TextOptions to use OCR
    TextOptions options = new TextOptions(false, true, ocrOptions);
    // Extract a text using OCR
    using (TextReader reader = parser.GetText(options))
    {
        // Print a text or 'not supported' message
        Console.WriteLine(reader == null ? "Text extraction isn't supported" : reader.ReadToEnd());
    }

    if (handler.HasWarnings)
    {
        Console.WriteLine("The following warnings occur while the text recognition:");

        foreach (string w in handler.Warnings)
        {
            Console.WriteLine("\\t* " + w);
        }
    }
    else
    {
        Console.WriteLine("the text recognition was performed without any warning.");
    }
}
```