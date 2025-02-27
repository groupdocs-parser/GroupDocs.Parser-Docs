---
id: extract-text-from-images
url: parser/net/extract-text-from-images
title: Extract a text from images and PDFs
weight: 3
description:  "This article explains how to extract a text from images and PDFs"
keywords: OCR, extract text from images
productName: GroupDocs.Parser for .NET
hideChildren: False
---
GroupDocs.Parser for .NET provides the ability to extract a text from images and PDFs (which don't contain a plain text) for English language.

{{< alert style="info" >}}To use the OCR functionality in .NET Framework set PlatformTarget to x64. If downloadable (msi or zip) version of GroupDocs.Parser is used, see readme.txt file for the additional information.{{< /alert >}}

The following example shows how to extract a text from images and PDFs:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser("scanned.pdf"))
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

TextOptions can be omitted if the file is an image:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser("scanned.jpg"))
{
    // Extract a text using OCR
    using(TextReader reader = parser.GetText())
    {
        // Print a text or 'not supported' message
        Console.WriteLine(reader == null ? "Text extraction isn't supported" : reader.ReadToEnd());
    }
}
```