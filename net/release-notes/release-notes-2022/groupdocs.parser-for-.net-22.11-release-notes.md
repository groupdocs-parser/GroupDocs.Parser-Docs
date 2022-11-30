---
id: groupdocs-parser-for-net-22-11-release-notes
url: parser/net/groupdocs-parser-for-net-22-11-release-notes
title: GroupDocs.Parser for .NET 22.11 Release Notes
weight: 1
description: ""
keywords: 
productName: GroupDocs.Parser for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Parser for .NET 22.11{{< /alert >}}

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| PARSERNET-1961 | Implement the ability to use OCR for images and PDF documents | New Feature |
| PARSERNET-1935 | Document info has 0 pages during reading JPEG file content | Bug |

## Public API and Backward Incompatible Changes

### Implement the ability to use OCR for images and PDF documents

#### Description

This feature provides the ability to extract a text and text areas using OCR.

#### Public API changes

[GroupDocs.Parser.Options.Features](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/features) public class was updated with changes as follows:

* Added [Preview](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/features/preview) and [Ocr](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/features/ocr) properties;

[GroupDocs.Parser.Options.PageTextAreaOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/pagetextareaoptions) public class was updated with changes as follows:

* Added [PageTextAreaOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/pagetextareaoptions/pagetextareaoptions#constructor)(bool) and [PageTextAreaOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/pagetextareaoptions/pagetextareaoptions#constructor_1)(bool, OcrOptions) constructors;
* Added [UseOcr](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/pagetextareaoptions/useocr) and [OcrOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/pagetextareaoptions/ocroptions) properties.

[GroupDocs.Parser.Options.TextOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/textoptions) public class was updated with changes as follows:

* Added [TextOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/textoptions/textoptions#constructor_1)(bool, bool) and [TextOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/textoptions/textoptions#constructor_2)(bool, bool, OcrOptions) constructors;
* Added [UseOcr](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/textoptions/useocr) and [OcrOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/textoptions/ocroptions) properties.

[GroupDocs.Parser.Options.ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings) public class was updated with changes as follows:

* Added [ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings/parsersettings#constructor_2)(OcrConnectorBase) and [ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings/parsersettings#constructor_1)(ILogger, OcrConnectorBase) constructors;
* Added [OcrConnector](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings/ocrconnector) property.

[GroupDocs.Parser.Options.Parser](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parser) public class was updated with changes as follows:

* Added [Parser](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/parser#constructor_11)(string, ParserSettings) and [Parser](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/parser#constructor_7)(Stream, ParserSettings) constructors;

[OcrConnectorBase](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocrconnectorbase), [OcrEventHandler](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocreventhandler), [OcrOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/ocroptions) classes were added into GroupDocs.Parser.Options namespace.

#### Usage

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

See [OCR Usage Basics]({{< ref "../../developer-guide/advanced-usage/using-ocr/ocr-usage-basics.md" >}}) for more details.