---
id: groupdocs-parser-for-net-23-3-release-notes
url: parser/net/groupdocs-parser-for-net-23-3-release-notes
title: GroupDocs.Parser for .NET 23.3 Release Notes
weight: 11
description: ""
keywords: 
productName: GroupDocs.Parser for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Parser for .NET 23.3{{< /alert >}}

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| PARSERNET-2074 | Improve PDF page preview rendering on Linux systems | Improvement |
| PARSERNET-2075 | Improve raw text extraction from PDF documents | Improvement |

## Public API and Backward Incompatible Changes

### Improve PDF page preview rendering on Linux systems

#### Description

This improvement enhanced generation PDF page preview on Linux systems.

#### Public API changes

No API changes.

#### Usage

The following example shows how to generate document page previews:

```csharp
// Create an instance of Parser class to generate document page previews
using (Parser parser = new Parser(Constants.SamplePdfWithToc))
{
    // Create preview options
    PreviewOptions previewOptions = new PreviewOptions(pageNumber => File.Create(GetOutputPath($"preview_{pageNumber}.png")));
    // Set PNG as an output image format
    previewOptions.PreviewFormat = PreviewFormats.PNG;
    // Set DPI for the output image
    previewOptions.Dpi = 72;

    // Generate previews
    parser.GeneratePreview(previewOptions);
}            

private static string GetOutputPath(string fileName)
{
    // Create the output directory if necessary
    if (!Directory.Exists(Constants.OutputPath))
    {
        Directory.CreateDirectory(Constants.OutputPath);
    }

    return Path.Combine(Constants.OutputPath, fileName);
}
```

### Improve raw text extraction from PDF documents

#### Description

This improvement enhanced RAW text extraction from PDF documents.

#### Public API changes

No API changes.

#### Usage

The following example shows how to extract a raw text from a document:

```csharp
// Create an instance of Parser class
using(Parser parser = new Parser(filePath))
{
    // Extract a raw text into the reader
    using(TextReader reader = parser.GetText(new TextOptions(true)))
    {
        // Print a text from the document
        // If text extraction isn't supported, a reader is null
        Console.WriteLine(reader == null ? "Text extraction isn't supported" : reader.ReadToEnd());
    }
}
```