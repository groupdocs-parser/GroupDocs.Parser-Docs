---
id: groupdocs-parser-for-java-22-11-release-notes
url: parser/java/groupdocs-parser-for-java-22-11-release-notes
title: GroupDocs.Parser for Java 22.11 Release Notes
weight: 1
description: ""
keywords: 
productName: GroupDocs.Parser for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Parser for Java 22.11{{< /alert >}}

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| PARSERNET-1961 | Implement the ability to use OCR for images and PDF documents | New Feature |
| PARSERNET-1903 | Implement the support for attachment extraction from presentations | New Feature |
| PARSERNET-1904 | Implement the support for attachment extraction from spreadsheets | New Feature |
| PARSERNET-1905 | Implement the support for attachment extraction from word processing documents | New Feature |

## Public API and Backward Incompatible Changes

### Implement the support for attachment extraction from presentations, spreadsheets and word processing documents

#### Description

These features provide the ability to extract attachments from documents.

#### Public API changes

No public API changes.

#### Usage

The following example shows how to extract a text from document attachments:

```Java
// Create an instance of Parser class
try (Parser parser = new Parser(fileName)) {
    // Extract attachments from the container
    Iterable<ContainerItem> attachments = parser.getContainer();
    // Check if container extraction is supported
    if (attachments == null) {
        System.out.println("Container extraction isn't supported");
    }
    // Iterate over zip entities
    for (ContainerItem item : attachments) {
        // Print the file path
        System.out.println(item.getFilePath());
        // Print metadata
        for (MetadataItem metadata : item.getMetadata()) {
            System.out.println(String.format("%s: %s", metadata.getName(), metadata.getValue()));
        }
        try {
            // Create Parser object for the zip entity content
            try (Parser attachmentParser = item.openParser()) {
                // Extract an zip entity text
                try (TextReader reader = attachmentParser.getText()) {
                    System.out.println(reader == null ? "No text" : reader.readToEnd());
                }
            }
        } catch (UnsupportedDocumentFormatException ex) {
            System.out.println("Isn't supported.");
        }
    }
}
```

### Implement the ability to use OCR for images and PDF documents

#### Description

This feature provides the ability to extract a text and text areas using OCR.

#### Public API changes

[GroupDocs.Parser.Options.Features](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/features) public class was updated with changes as follows:

* Added [isPreview](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/features/#isPreview--) and [isOcr](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/features/#isOcr--) properties;

[GroupDocs.Parser.Options.PageTextAreaOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/pagetextareaoptions) public class was updated with changes as follows:

* Added [PageTextAreaOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/pagetextareaoptions/#PageTextAreaOptions-boolean-)(bool) and [PageTextAreaOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/pagetextareaoptions/#PageTextAreaOptions-boolean-com.groupdocs.parser.options.OcrOptions-)(bool, OcrOptions) constructors;
* Added [isUseOcr](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/pagetextareaoptions/#isUseOcr--) and [OcrOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/pagetextareaoptions/#getOcrOptions--) properties.

[GroupDocs.Parser.Options.TextOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/textoptions) public class was updated with changes as follows:

* Added [TextOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/textoptions/#TextOptions-boolean-boolean-)(bool, bool) and [TextOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/textoptions/#TextOptions-boolean-boolean-com.groupdocs.parser.options.OcrOptions-)(bool, bool, OcrOptions) constructors;
* Added [isUseOcr](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/textoptions/#isUseOcr--) and [OcrOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/textoptions/#getOcrOptions--) properties.

[GroupDocs.Parser.Options.ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings) public class was updated with changes as follows:

* Added [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings/#ParserSettings-com.groupdocs.parser.options.OcrConnectorBase-)(OcrConnectorBase) and [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings/#ParserSettings-com.groupdocs.parser.options.ILogger-com.groupdocs.parser.options.OcrConnectorBase-)(ILogger, OcrConnectorBase) constructors;
* Added [OcrConnector](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings/#getOcrConnector--) property.

[GroupDocs.Parser.Options.Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parser) public class was updated with changes as follows:

* Added [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#Parser-java.lang.String-com.groupdocs.parser.options.ParserSettings-)(string, ParserSettings) and [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#Parser-java.io.InputStream-com.groupdocs.parser.options.ParserSettings-)(Stream, ParserSettings) constructors;

[OcrConnectorBase](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocrconnectorbase), [OcrEventHandler](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocreventhandler), [OcrOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ocroptions) classes were added into GroupDocs.Parser.Options namespace.

#### Usage

The following example shows how to extract a text from the image file:

```C#
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

See [OCR Usage Basics]({{< ref "../../developer-guide/advanced-usage/using-ocr/ocr-usage-basics.md" >}}) for more details.