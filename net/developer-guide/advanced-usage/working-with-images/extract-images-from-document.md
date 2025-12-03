---
id: extract-images-from-document
url: parser/net/extract-images-from-document
title: Extract images from document
weight: 1
version: 23.5
description: "Learn how to extract images from documents using GroupDocs.Parser for .NET. Extract images with position data, rotation, and format information from PDF, Word, Excel in C#."
keywords: extract images from document, extract images, extract images from PDF, extract images from DOCX, image extraction with coordinates, document image extraction C#
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
tags: csharp, parser, image-extraction, images, v23.5
---
GroupDocs.Parser provides the functionality to extract images from documents by the [GetImages](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getimages) method:

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
| [Save(string)](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pageimagearea/methods/save) | Saves the image to the file. |
| [Save(string, ImageOptions)](https://reference.groupdocs.com/net/parser/groupdocs.parser.data.pageimagearea/save/methods/1) | Saves the image to the file in a different format. |

[ImageOptions](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/imageoptions) class is used to define the image format into which the image is converted. The following image formats are supported:

*   Bmp
*   Gif
*   Jpeg
*   Png
*   WebP

Here are the steps to extract images from the whole document:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Call [GetImages](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getimages) method and obtain collection of [PageImageArea](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pageimagearea) objects;
*   Check if *collection* isn't *null* (images extraction is supported for the document);
*   Iterate through the collection and get sizes, image types and image contents.

The following example shows how to extract all images from a document with comprehensive error handling:

```csharp
using GroupDocs.Parser;
using GroupDocs.Parser.Data;
using GroupDocs.Parser.Exceptions;
using System;
using System.Collections.Generic;
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
        
        Console.WriteLine($"Found {imagesList.Count} image(s) in the document.\n");
        
        // Iterate over images
        int imageIndex = 0;
        foreach (PageImageArea image in imagesList)
        {
            imageIndex++;
            Console.WriteLine($"=== Image {imageIndex} ===");
            Console.WriteLine($"Page: {image.Page.Index + 1}");
            Console.WriteLine($"Position: X={image.Rectangle.X}, Y={image.Rectangle.Y}");
            Console.WriteLine($"Size: Width={image.Rectangle.Width}, Height={image.Rectangle.Height}");
            Console.WriteLine($"Rotation: {image.Rotation}°");
            Console.WriteLine($"Format: {image.FileType}");
            Console.WriteLine();
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

### Example: Extract and Save Images to Files

This example demonstrates how to extract images and save them to disk:

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
            
            // Get image position information (coordinates)
            Console.WriteLine($"Extracting image {imageIndex} from page {image.Page.Index + 1}");
            Console.WriteLine($"  Position: ({image.Rectangle.X}, {image.Rectangle.Y})");
            Console.WriteLine($"  Size: {image.Rectangle.Width}x{image.Rectangle.Height}");
            
            // Save image preserving original format
            string imagePath = Path.Combine(
                outputDir, 
                $"image_page_{image.Page.Index + 1}_#{imageIndex}.{image.FileType.Extension}"
            );
            
            image.Save(imagePath);
            Console.WriteLine($"  Saved to: {imagePath}\n");
        }
    }
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online image extractor App

Along with full featured .NET library we provide simple, but powerfull free APPs.

You are welcome to extract images from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [GroupDocs Parser App](https://products.groupdocs.app/parser).