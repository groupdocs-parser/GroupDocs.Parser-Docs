---
id: groupdocs-parser-for-java-23-2-release-notes
url: parser/java/groupdocs-parser-for-java-23-2-release-notes
title: GroupDocs.Parser for Java 23.2 Release Notes
weight: 12
description: ""
keywords: 
productName: GroupDocs.Parser for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Parser for Java 23.2{{< /alert >}}

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| PARSERNET-1991 | Implement the ability to handle loading of external resources | New Feature |

## Public API and Backward Incompatible Changes

### Implement the ability to handle loading of external resources

#### Description

This feature provides the ability to handle loading of HTML external resource.

#### Public API changes

[ExternalResourceHandler](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/externalresourcehandler/) public class was added

[ExternalResourceLoadingArgs](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/externalresourceloadingargs/) public class was added

[ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings/) public class was updated with changes as follows:

* Added [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings/#ParserSettings-com.groupdocs.parser.options.ExternalResourceHandler-)(ExternalResourceHandler) and [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings/#ParserSettings-com.groupdocs.parser.options.ILogger-com.groupdocs.parser.options.OcrConnectorBase-com.groupdocs.parser.options.ExternalResourceHandler-)(ILogger, OcrConnectorBase, ExternalResourceHandler) constructors;

#### Usage

The following example shows how to handle loading of HTML external resource:

```java
// Create an instance of ParserSettings to pass External Resource Handler
ParserSettings settings = new ParserSettings(new Handler());
// Create an instance of Parser class to generate spreadsheet page previews
try (Parser parser = new Parser(Constants.SampleHtmlWithImages, settings)) {
    // Extract images from HTML document
    Iterable<PageImageArea> images = parser.getImages();
    // Iterate over extracted images
    for (PageImageArea i : images) {
        // Print the type of image
        System.out.println(i.getFileType());
    }
}

/**
 * This class provides the ability to filter extracted images.
 **/
class Handler extends ExternalResourceHandler {
    // Called before any external resource loads. It allows to skip unnecessary file loading.
    @Override
    public void onLoading(ExternalResourceLoadingArgs args) {
        // Check if the file name ends with installation.png
        if (!args.getUri().endsWith("installation.png")) {
            // Otherwise skip this file
            args.setSkipped(true);
        }

        super.onLoading(args);
    }
}
```