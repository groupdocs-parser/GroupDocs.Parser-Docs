---
id: extract-text-from-images
url: parser/net/extract-text-from-images
title: Extract a text from images and PDFs
weight: 3
description:  "This article explains how to extract text from images and PDFs"
keywords: OCR, extract text from images
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---
GroupDocs.Parser for .NET provides the ability to extract text from image files and PDFs composed of images.

{{< alert style="info" >}}To use the OCR functionality in .NET Framework set PlatformTarget to x64. If downloadable (msi or zip) version of GroupDocs.Parser is used, see readme.txt file for the additional information.{{< /alert >}}

It should be noted that not all languages ​​represented by the [Language](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/language/) class are currently supported for recognition without implicitly downloading additional resources from the internet. However, if internet access is available, all necessary resources will be downloaded implicitly when selecting any recognition language. Currently supported languages ​​without additional downloads: English, Chinese, Japanese, Korean, Arabic.

The following example shows how to extract text from images and PDFs:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser("scanned.pdf"))
{
    // Set OCR options
    TextOptions options = new TextOptions(false, true);
    options.OcrOptions = new OcrOptions();
    options.OcrOptions.Language = Language.Chinese;
    options.OcrOptions.PagePreviewOptions = new PagePreviewOptions();
    options.OcrOptions.PagePreviewOptions.Dpi = 144;
    // Extract text using OCR
    using(TextReader reader = parser.GetText(options))
    {
        // Print text or 'not supported' message
        Console.WriteLine(reader == null ? "Text extraction isn't supported" : reader.ReadToEnd());
    }
}
```

TextOptions can be omitted if the file is an image:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser("scanned.jpg"))
{
    // Extract text using OCR
    using(TextReader reader = parser.GetText())
    {
        // Print text or 'not supported' message
        Console.WriteLine(reader == null ? "Text extraction isn't supported" : reader.ReadToEnd());
    }
}
```