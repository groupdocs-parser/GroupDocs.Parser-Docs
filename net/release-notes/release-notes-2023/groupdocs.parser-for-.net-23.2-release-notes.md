---
id: groupdocs-parser-for-net-23-2-release-notes
url: parser/net/groupdocs-parser-for-net-23-2-release-notes
title: GroupDocs.Parser for .NET 23.2 Release Notes
weight: 12
description: ""
keywords: 
productName: GroupDocs.Parser for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Parser for .NET 23.2{{< /alert >}}

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| PARSERNET-1991 | Implement the ability to handle loading of external resources | New Feature |

## Public API and Backward Incompatible Changes

### Implement the ability to handle loading of external resources

#### Description

This feature provides the ability to handle loading of HTML external resource.

#### Public API changes

[ExternalResourceHandler](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/externalresourcehandler/) public class was added

[ExternalResourceLoadingArgs](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/externalresourceloadingargs/) public class was added

[ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings) public class was updated with changes as follows:

* Added [ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings/parsersettings#constructor)(ExternalResourceHandler) and [ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings/parsersettings#constructor_3)(ILogger, OcrConnectorBase, ExternalResourceHandler) constructors;

#### Usage

The following example shows how to handle loading of HTML external resource:

```csharp
// Create an instance of ParserSettings to pass External Resource Handler
ParserSettings settings = new ParserSettings(new Handler());

// Create an instance of Parser class to generate spreadsheet page previews
using (Parser parser = new Parser(Constants.SampleHtmlWithImages, settings))
{
    // Extract images from HTML document
    IEnumerable<PageImageArea> images = parser.GetImages();

    // Iterate over extracted images
    foreach (PageImageArea i in images)
    {
        // Print the type of image
        Console.WriteLine(i.FileType);
    }
}

/// <summary>
/// This class provides the ability to filter extracted images.
/// </summary>
private class Handler : ExternalResourceHandler
{
    /// <summary>
    /// Called before any external resource loads. It allows to skip unnesesary file loading.
    /// </summary>
    public override void OnLoading(ExternalResourceLoadingArgs args)
    {
        // Check if the file name ends with installation.png
        if (!args.Uri.EndsWith("installation.png"))
        {
            // Otherwise skip this file
            args.Skipped = true;
        }

        base.OnLoading(args);
    }
}
```