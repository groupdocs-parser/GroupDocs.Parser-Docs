---
id: extract-images-from-documents
url: parser/net/extract-images-from-documents
title: Extract images from documents
weight: 8
version: 23.5
description: "Learn how to extract images from PDF, Word, Excel, PowerPoint and 50+ document formats using GroupDocs.Parser for .NET. Extract images with position data, coordinates, and save to files in C#."
keywords: extract images, extract images from pdf, extract images from DOCX, image extraction C#, extract images with coordinates, document image extraction
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
tags: csharp, parser, image-extraction, v23.5
---
GroupDocs.Parser allows to extract images from PDF, Emails, Ebooks, Microsoft Office: Word (DOC, DOCX), PowerPoint (PPT, PPTX), Excel (XLS, XLSX), LibreOffice formats and many others (see full list at [supported document formats]({{< ref "parser/net/getting-started/supported-document-formats.md" >}}) article).

GroupDocs.Parser's allows to easily implement simple and complex image extraction cases at the same time (see more at [advanced help section]({{< ref "parser/net/developer-guide/advanced-usage/working-with-images/_index.md" >}})).

In this article you can see how to extract images from any supported format without additional settings.

## Extract images from documents

To extract images from documents simply call [GetImages](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getimages) method:

```csharp
IEnumerable<PageImageArea> GetImages();

```

The methods return a collection of [PageImageArea](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pageimagearea) objects:

| Member | Description |
| --- | --- |
| [Page](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagearea/properties/page) | The page that contains the text area. |
| [Rectangle](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagearea/properties/rectangle) | The rectangular area on the page that contains the text area. |
| [FileType](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pageimagearea/properties/filetype) | The format of the image. |
| [Rotation](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pageimagearea/properties/rotation) | The rotation angle of the image. |
| Stream [GetImageStream()](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pageimagearea/methods/getimagestream) | Returns the image stream. |
| Stream [GetImageStream(ImageOptions)](https://reference.groupdocs.com/net/parser/groupdocs.parser.data.pageimagearea/getimagestream/methods/1) | Returns the image stream in a different format. |
| void [Save(String)](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pageimagearea/methods/save) | Saves the image to the file. |
| void Save(String, ImageOptions) | Saves the image to the file in a different format. |

Here are the steps to extract images from the whole document:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser)  object for the initial document;
*   Call [GetImages](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getimages) method and obtain collection of image objects;
*   Check if *collection* isn't null (images extraction is supported for the document);
*   Iterate through the collection and get sizes, image types and image contents.

The following example shows how to extract all images from a document with error handling:

```csharp
using GroupDocs.Parser;
using GroupDocs.Parser.Data;
using GroupDocs.Parser.Exceptions;
using System;
using System.IO;
using System.Linq;

try
{
    // Create an instance of Parser class
    using (Parser parser = new Parser(filePath))
    {
        // Extract images
        IEnumerable<PageImageArea> images = parser.GetImages();
        
        // Check if image extraction is supported
        if (images == null)
        {
            Console.WriteLine("Image extraction isn't supported for this document format.");
            return;
        }
        
        var imagesList = images.ToList();
        
        if (imagesList.Count == 0)
        {
            Console.WriteLine("No images found in this document.");
            return;
        }
        
        Console.WriteLine($"Found {imagesList.Count} image(s) in the document.");
        
        // Iterate over images
        int imageIndex = 0;
        foreach (PageImageArea image in imagesList)
        {
            imageIndex++;
            // Print page index, rectangle position, and image type
            Console.WriteLine($"Image {imageIndex}: Page {image.Page.Index + 1}, " +
                            $"Position: {image.Rectangle}, Type: {image.FileType}");
        }
    }
}
catch (FileNotFoundException)
{
    Console.WriteLine($"Error: File not found at path '{filePath}'");
}
catch (ParserException ex)
{
    Console.WriteLine($"Error extracting images: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Unexpected error: {ex.Message}");
}
```

### Example: Extract Images with Coordinates and Save to Files

This example demonstrates how to extract images with position information and save them to disk:

```csharp
using GroupDocs.Parser;
using GroupDocs.Parser.Data;
using System;
using System.IO;

using (Parser parser = new Parser(filePath))
{
    var images = parser.GetImages();
    
    if (images != null)
    {
        string outputDir = "extracted_images";
        Directory.CreateDirectory(outputDir);
        
        int imageIndex = 0;
        foreach (PageImageArea image in images)
        {
            imageIndex++;
            
            // Get image position information
            Console.WriteLine($"Image {imageIndex}:");
            Console.WriteLine($"  Page: {image.Page.Index + 1}");
            Console.WriteLine($"  Position: X={image.Rectangle.X}, Y={image.Rectangle.Y}");
            Console.WriteLine($"  Size: Width={image.Rectangle.Width}, Height={image.Rectangle.Height}");
            Console.WriteLine($"  Rotation: {image.Rotation}°");
            Console.WriteLine($"  Format: {image.FileType}");
            
            // Save image to file preserving original format
            string imagePath = Path.Combine(outputDir, $"image_{imageIndex}.{image.FileType.Extension}");
            image.Save(imagePath);
            Console.WriteLine($"  Saved to: {imagePath}\n");
        }
    }
}
```

{{< alert style="info" >}}
**Note:** The `PageImageArea` object contains `Rectangle` property that provides the position (X, Y coordinates) and size (Width, Height) of the image on the page. This is useful for preserving the document layout or extracting images from specific regions.
{{< /alert >}}

## More resources

### Advanced usage topics

To learn more about image extraction feature, please refer the [advanced help section]({{< ref "parser/net/developer-guide/advanced-usage/working-with-images/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online image extractor App

Along with full featured .NET library we provide simple, but powerfull free APPs.

You are welcome to extract images from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [GroupDocs Parser App](https://products.groupdocs.app/parser).