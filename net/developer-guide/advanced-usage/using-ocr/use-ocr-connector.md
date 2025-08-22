---
id: use-ocr-connector
url: parser/net/use-ocr-connector
title: Use OCR Connector
weight: 2
description:  "This article explains how to integrate OCR solution to GroupDocs.Parser"
keywords: OCR, extract text from images
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---
GroupDocs.Parser provides out-of-the-box text recognition from images and scanned PDFs in English. This requires no additional setup; you just need to explicitly request OCR when extracting text or parsing a document. Moreover, for image files, even this is not necessary—OCR will be automatically applied during text extraction without any extra reminders.

If the capabilities of the built-in OCR are not sufficient, you can use the mechanism to connect a third-party OCR. To do this, you need to write your own OCR connector and pass it when creating the Parser class:

* Instantiate [ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings) object with the instance of class that implements OCR functionality;
* Instantiate [Parser](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/) object with [ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings) object.

The following example shows how to create an instance of [Parser](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/) class with the custom connector:

```C#
// Create an instance of ParserSettings class with the custom ocr connector
ParserSettings settings = new ParserSettings(new YourCustomOcrConnector());
// Create an instance of Parser class with the parser settings
Parser parser = new Parser(fileName, settins);
```

All OCR connectors inherit from the base class [OcrConnectorBase](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocrconnectorbase/) (including the built-in one in the parser). This class contains two properties and three methods:

```C#
public abstract class OcrConnectorBase
{
    public virtual bool IsTextSupported { get; }

    public virtual bool IsTextAreasSupported { get; }

    public virtual string RecognizeText(Stream imageStream, OcrOptions options);

    public virtual IList<PageTextArea> RecognizeTextAreas(Stream imageStream, Page page, OcrOptions options);

    public virtual IList<PageTextArea> RecognizeTextAreas(Stream imageStream, IEnumerable<Rectangle> rectangles, string allowedSymbols, Page page, OcrOptions options);
}
```

Parser separates text recognition into two independent workflows: plain text and text areas. The first one handles extracting text from a document, which can be obtained, for example, via Parser.GetText. Text areas are used in the template-based parsing functionality. These two workflows are independent of each other, and when creating a custom connector, you only need to implement the required functionality.

The IsTextSupported property answers the question: Can the connector extract plain text? If it returns false, there's no need to override the RecognizeText method, as Parser won't call it and will immediately return null when text is requested.

If this property returns true, then you need to implement the RecognizeText method. It takes two parameters: a stream with the image (imageStream) and OCR settings (options).

The OCR settings are represented by the [OcrOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocroptions/) class and may include:

* [OcrEventHandler](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocroptions/handler) for sending messages that occur during document processing.
* A [rectangular area](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocroptions/rectangle) of the image from which text needs to be extracted; in this case, there's no need to extract text from the entire image, limiting it to the specified area.
* A [flag](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocroptions/usespellchecker) for using spell checking (if supported by the OCR).

The IsTextAreasSupported property answers the question: Can the connector extract text areas? Text areas are rectangles on a page that contain text. Typically, these areas represent individual words (this assumption is the basis for the template-based parsing functionality). If the property returns false, it means this functionality is not supported, and no further action is required.

If this property returns true, then you need to implement the RecognizeTextAreas methods. In addition to the image and OCR settings, these methods receive the [document's page class](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/page) and, in a more advanced version, a list of rectangular areas to recognize and a list of allowed characters.

Both methods should return a collection of [PageTextArea](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagetextarea) objects. These objects contain the document's page, the text, and the rectangular area on the page where the text was found. You can pass the page parameter as the page.

The coordinates of the rectangles are calculated based on the Rectangle value from [OcrOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocroptions). This means that if the rectangle from the OCR settings has the top-left corner at (10; 10), and the text on the page has coordinates (12; 15), then you should return (2; 5). In other words, the image is first cropped to the rectangle, and then OCR is performed.

It's important to note that these two methods are independent, and you need to implement both of them.

The list of rectangular areas specifies the coordinates within the already cropped image from which text needs to be extracted. This functionality is used in template-based parsing for simple fields—the user defines a field using a rectangle. The list of these rectangles is then sent to the connector to extract text data based on these coordinates.

The list of allowed characters is a string containing the characters that Parser expects to see in these areas. This can be used, for example, to extract only digits (if there is ambiguity between "O" and "0," the system will favor the latter) or to explicitly define decimal separators (e.g., specifying a dot instead of a comma) and so on. Currently, this functionality is not supported, and the parameter has been added for potential expansion in future releases.