---
id: extract-images-from-document
url: parser/python-net/extract-images-from-document
title: Extract images from document
weight: 1
description: "Learn how to extract images from documents using GroupDocs.Parser for Python via .NET. Extract images with position data, rotation, and format information from PDF, Word, Excel."
keywords: extract images from document, extract images, extract images from PDF, extract images from DOCX, image extraction with coordinates, document image extraction
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser provides functionality to extract images from various document formats including PDF, Word, Excel, PowerPoint, and more.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample documents containing images
- Write access to save extracted images (optional)

## Extract images from document

To extract all images from a document:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Extract images
    images = parser.get_images()
    
    # Check if image extraction is supported
    if images is None:
        print("Image extraction isn't supported")
    elif not images:
        print("No images found")
    else:
        # Iterate over images
        for idx, image in enumerate(images):
            # Print image information
            print(f"Image {idx + 1}:")
            print(f"  Page: {image.page.index + 1}")
            print(f"  Type: {image.file_type}")
            print(f"  Size: {image.rectangle.size.width}x{image.rectangle.size.height}")
            print(f"  Position: ({image.rectangle.left}, {image.rectangle.top})")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "extract-images-from-document.txt" >}}  
```text
Image 1:
  Page: 1
  Type: JPEG Image(.jpeg)
  Size: 150x45
  Position: (40, 70.99999999593751)
Image 2:
  Page: 1
  Type: JPEG Image(.jpeg)
  Size: 375x234
  Position: (110, 383.9999999959375)
[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document/example-1/extract-images-from-document.txt)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns a collection of `PageImageArea` objects representing all images found in the document, or `None` if image extraction is not supported.

## Save extracted images to files

To save extracted images to disk:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import os

# Create output directory
output_dir = "extracted_images"
os.makedirs(output_dir, exist_ok=True)

# Create an instance of Parser class
with Parser("./sample.docx") as parser:
    # Extract images
    images = parser.get_images()
    
    if images is None:
        print("Image extraction isn't supported")
    elif not images:
        print("No images found")
    else:
        # Iterate over images and save them
        for idx, image in enumerate(images):
            # Get file extension based on image type
            extension = image.file_type.extension
            
            # Generate filename
            filename = f"image_{idx + 1}{extension}"
            filepath = os.path.join(output_dir, filename)
            
            # Save image to file
            image.save(filepath)
            print(f"Saved: {filepath}")
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "save-extracted-images-to-files-outputs.zip" >}}  
```text
extracted_images/image_1.png (4 KB)
extracted_images/image_2.jpeg (176 KB)
extracted_images/image_3.jpeg (136 KB)
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document/example-2/save-extracted-images-to-files-outputs.zip)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Saves each extracted image to a separate file with the appropriate file extension (.png, .jpg, .gif, etc.).

## Extract images with metadata

To extract images along with detailed metadata:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.pptx") as parser:
    # Check if image extraction is supported
    if not parser.features.images:
        print("Document doesn't support image extraction")
    else:
        # Extract images
        images = parser.get_images()
    
        if images:
            print(f"Found {len(images)} images\n")

            for idx, image in enumerate(images):
                print(f"Image {idx + 1}:")
                print(f"  Page: {image.page.index + 1}")
                print(f"  Format: {image.file_type}")
                print(f"  Rotation: {image.rotation} degrees")
                print(f"  Rectangle: {image.rectangle}")
                print(f"  Width: {image.rectangle.size.width}")
                print(f"  Height: {image.rectangle.size.height}")
                print()
        else:
            print("No images found")
```

{{< /tab >}}
{{< tab "sample.pptx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pptx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document/sample.pptx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "extract-images-with-metadata.txt" >}}  
```text
Found 4 images

Image 1:
  Page: 1
  Format: Portable Network Graphic(.png)
  Rotation: 0 degrees
  Rectangle: (30; 20) (150; 45)
  Width: 150
  Height: 45

[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document/example-3/extract-images-with-metadata.txt)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Displays comprehensive information about each image including position, size, format, and rotation angle.

## Get image stream

To work with image data as a stream:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import ImageOptions, ImageFormat

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Extract images
    images = parser.get_images()
    
    if images:
        for idx, image in enumerate(images):
            # Get image stream (a .NET MemoryStream)
            image_stream = image.get_image_stream()
            
            # Read image data: to_array() returns the stream content as bytes
            image_data = image_stream.to_array()
            print(f"Image {idx + 1}: {len(image_data)} bytes, format: {image.file_type}")
            
            # Optionally convert to PNG
            png_options = ImageOptions(ImageFormat.PNG)
            png_stream = image.get_image_stream(png_options)
            png_data = png_stream.to_array()
            print(f"  Converted to PNG: {len(png_data)} bytes")
    else:
        print("No images found")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "get-image-stream.txt" >}}  
```text
Image 1: 10365 bytes, format: JPEG Image(.jpeg)
  Converted to PNG: 55430 bytes
Image 2: 90027 bytes, format: JPEG Image(.jpeg)
  Converted to PNG: 633711 bytes
Image 3: 125180 bytes, format: JPEG Image(.jpeg)
  Converted to PNG: 837256 bytes
Image 4: 163435 bytes, format: JPEG Image(.jpeg)
  Converted to PNG: 1186275 bytes
Image 5: 94989 bytes, format: JPEG Image(.jpeg)
  Converted to PNG: 928726 bytes
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document/example-4/get-image-stream.txt)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Provides access to raw image data as a stream, with optional format conversion.

## Convert images during extraction

To convert images to a specific format during extraction:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import ImageOptions, ImageFormat
import os

# Create output directory
output_dir = "converted_images"
os.makedirs(output_dir, exist_ok=True)

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Extract images
    images = parser.get_images()
    
    if images:
        # Create image options for PNG format
        png_options = ImageOptions(ImageFormat.PNG)
        
        for idx, image in enumerate(images):
            # Save image as PNG (regardless of original format)
            filename = f"image_{idx + 1}.png"
            filepath = os.path.join(output_dir, filename)
            
            # Save with conversion
            image.save(filepath, png_options)
            print(f"Saved as PNG: {filepath}")
    else:
        print("No images found")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "convert-images-during-extraction-outputs.zip" >}}  
```text
converted_images/image_1.png (54 KB)
converted_images/image_2.png (619 KB)
converted_images/image_3.png (818 KB)
converted_images/image_4.png (1158 KB)
converted_images/image_5.png (907 KB)
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document/example-5/convert-images-during-extraction-outputs.zip)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** All extracted images are converted to PNG format before saving, regardless of their original format.

## Batch image extraction

Extract images from multiple documents:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import os
from pathlib import Path

def extract_images_from_directory(input_dir, output_dir):
    """
    Extract images from all documents in a directory.
    """
    os.makedirs(output_dir, exist_ok=True)
    
    # Supported document extensions
    extensions = ['.pdf', '.docx', '.doc', '.xlsx', '.pptx', '.ppt']
    
    for file_path in sorted(Path(input_dir).rglob('*')):
        if file_path.suffix.lower() in extensions:
            print(f"\nProcessing: {file_path.name}")
            
            try:
                with Parser(str(file_path)) as parser:
                    images = parser.get_images()
                    
                    if images is None:
                        print(f"  Image extraction not supported")
                        continue
                    
                    # Create subdirectory for this document
                    doc_output_dir = os.path.join(output_dir, file_path.stem)
                    os.makedirs(doc_output_dir, exist_ok=True)
                    
                    # Save images
                    image_count = 0
                    for idx, image in enumerate(images):
                        filename = f"image_{idx + 1}{image.file_type.extension}"
                        filepath = os.path.join(doc_output_dir, filename)
                        image.save(filepath)
                        image_count += 1
                    
                    print(f"  Extracted {image_count} images")
                    
            except Exception as e:
                print(f"  Error: {e}")

# Usage
extract_images_from_directory("documents", "extracted_images")
```

{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [annual-report.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document/documents/annual-report.pdf), [product-catalog.pptx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document/documents/product-catalog.pptx), [service-agreement.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document/documents/service-agreement.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Notes

- The `get_images()` method returns `None` if image extraction is not supported for the document format
- Always check `parser.features.images` before attempting to extract images
- Supported output formats: BMP, GIF, JPEG, PNG, WebP
- Images are extracted with their original quality and resolution
- The `rotation` property indicates the image rotation angle (0, 90, 180, or 270 degrees)
- Use `get_image_stream()` for memory-efficient processing of large images

## Related pages

- [Extract images from document page]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page" >}})
- [Extract images from document page area]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page-area" >}})
- [Extract images to files]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-images/extract-images-to-files" >}})

