---
id: extract-images-to-files
url: parser/python-net/extract-images-to-files
title: Extract images to files
weight: 4
description: "Learn how to save extracted images to files using GroupDocs.Parser for Python via .NET. Save images in different formats with format conversion."
keywords: extract images to files, save images, image format conversion
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser provides functionality to save extracted images directly to files with support for format conversion.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample documents with images
- Write permissions to output directory

## Save images to files

To extract and save images to files:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import os

# Create output directory
output_dir = "extracted_images"
os.makedirs(output_dir, exist_ok=True)

# Create an instance of Parser class
with Parser("./document.pdf") as parser:
    # Extract images
    images = parser.get_images()
    
    # Check if image extraction is supported
    if images is None:
        print("Image extraction isn't supported")
    else:
        # Iterate over images and save them
        for idx, image in enumerate(images):
            # Generate filename with appropriate extension
            filename = f"image_{idx + 1}{image.file_type.extension}"
            filepath = os.path.join(output_dir, filename)
            
            # Save image to file
            image.save(filepath)
            print(f"Saved: {filename}")
```

{{< /tab >}}
{{< tab "document.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-to-files/document.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Saves each extracted image to a separate file using the image's original format.

## Save images with format conversion

To save images in a specific format (e.g., PNG):
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import ImageOptions, ImageFormat
import os

# Create output directory
output_dir = "images_png"
os.makedirs(output_dir, exist_ok=True)

# Create an instance of Parser class
with Parser("presentation.pptx") as parser:
    # Extract images
    images = parser.get_images()
    
    if images is None:
        print("Image extraction not supported")
    else:
        # Create image options for PNG format
        png_options = ImageOptions(ImageFormat.PNG)
        
        # Save all images as PNG
        for idx, image in enumerate(images):
            filename = f"image_{idx + 1}.png"
            filepath = os.path.join(output_dir, filename)
            
            # Save with format conversion
            image.save(filepath, png_options)
            print(f"Saved as PNG: {filename}")
```

{{< /tab >}}
{{< tab "presentation.pptx" >}}
{{< tab-text >}}
The following sample file is used in this example: [presentation.pptx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-to-files/presentation.pptx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "ImageFormat.PNG" >}}
{{< tab-text >}}
The following sample file is used in this example: [ImageFormat.PNG](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-to-files/ImageFormat.PNG)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Converts all extracted images to PNG format before saving.

## Save images with custom naming

To save images with descriptive filenames:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import os
from datetime import datetime

def save_images_with_custom_names(file_path, output_dir, prefix="doc"):
    """
    Save images with custom naming pattern.
    """
    os.makedirs(output_dir, exist_ok=True)
    
    # Generate timestamp
    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    
    with Parser(file_path) as parser:
        images = parser.get_images()
        
        if images is None:
            print("Image extraction not supported")
            return 0
        
        saved_count = 0
        
        for idx, image in enumerate(images):
            # Create custom filename
            filename = f"{prefix}_{timestamp}_page{image.page.index + 1}_img{idx + 1}{image.file_type.extension}"
            filepath = os.path.join(output_dir, filename)
            
            # Save image
            image.save(filepath)
            print(f"Saved: {filename}")
            saved_count += 1
        
        return saved_count

# Usage
count = save_images_with_custom_names("report.pdf", "saved_images", prefix="report")
print(f"
Total images saved: {count}")
```

{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Saves images with descriptive filenames including timestamp, page number, and index.

## Save images in multiple formats

To save each image in multiple formats:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import ImageOptions, ImageFormat
import os

# Create output directory
output_dir = "images_multi_format"
os.makedirs(output_dir, exist_ok=True)

# Create an instance of Parser class
with Parser("document.docx") as parser:
    images = parser.get_images()
    
    if images:
        # Define formats to save
        formats = {
            'png': ImageOptions(ImageFormat.PNG),
            'jpg': ImageOptions(ImageFormat.JPEG),
            'bmp': ImageOptions(ImageFormat.BMP)
        }
        
        for idx, image in enumerate(images):
            print(f"
Processing image {idx + 1}:")
            
            # Save in each format
            for format_name, options in formats.items():
                filename = f"image_{idx + 1}.{format_name}"
                filepath = os.path.join(output_dir, filename)
                
                image.save(filepath, options)
                print(f"  Saved as {format_name.upper()}")
```

{{< /tab >}}
{{< tab "document.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-to-files/document.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "ImageFormat.PNG" >}}
{{< tab-text >}}
The following sample file is used in this example: [ImageFormat.PNG](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-to-files/ImageFormat.PNG)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "ImageFormat.JPEG" >}}
{{< tab-text >}}
The following sample file is used in this example: [ImageFormat.JPEG](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-to-files/ImageFormat.JPEG)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "ImageFormat.BMP" >}}
{{< tab-text >}}
The following sample file is used in this example: [ImageFormat.BMP](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-to-files/ImageFormat.BMP)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Saves each image in PNG, JPEG, and BMP formats.

## Organize images by page

To save images organized by page number:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import os

def save_images_by_page(file_path, output_dir):
    """
    Save images organized in subdirectories by page.
    """
    os.makedirs(output_dir, exist_ok=True)
    
    with Parser(file_path) as parser:
        images = parser.get_images()
        
        if images is None:
            print("Image extraction not supported")
            return
        
        # Group images by page
        images_by_page = {}
        
        for image in images:
            page_num = image.page.index + 1
            if page_num not in images_by_page:
                images_by_page[page_num] = []
            images_by_page[page_num].append(image)
        
        # Save images organized by page
        total_saved = 0
        
        for page_num, page_images in sorted(images_by_page.items()):
            # Create page directory
            page_dir = os.path.join(output_dir, f"page_{page_num}")
            os.makedirs(page_dir, exist_ok=True)
            
            print(f"
Page {page_num}: {len(page_images)} images")
            
            for img_idx, image in enumerate(page_images):
                filename = f"image_{img_idx + 1}{image.file_type.extension}"
                filepath = os.path.join(page_dir, filename)
                
                image.save(filepath)
                print(f"  Saved: {filename}")
                total_saved += 1
        
        print(f"
Total images saved: {total_saved}")

# Usage
save_images_by_page("multi_page.pdf", "images_by_page")
```

{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Creates subdirectories for each page and saves images within their respective page folders.

## Save only images of specific type

To save only images of a particular format:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import os

def save_images_by_type(file_path, output_dir, target_extensions):
    """
    Save only images of specific types.
    
    Args:
        file_path: Path to document
        output_dir: Output directory
        target_extensions: List of extensions to save (e.g., ['.jpg', '.png'])
    """
    os.makedirs(output_dir, exist_ok=True)
    
    with Parser(file_path) as parser:
        images = parser.get_images()
        
        if images is None:
            print("Image extraction not supported")
            return
        
        saved_count = 0
        skipped_count = 0
        
        for idx, image in enumerate(images):
            ext = image.file_type.extension.lower()
            
            if ext in target_extensions:
                filename = f"image_{idx + 1}{ext}"
                filepath = os.path.join(output_dir, filename)
                
                image.save(filepath)
                print(f"Saved: {filename}")
                saved_count += 1
            else:
                print(f"Skipped: image_{idx + 1}{ext}")
                skipped_count += 1
        
        print(f"
Saved: {saved_count}, Skipped: {skipped_count}")

# Usage - save only JPG and PNG images
save_images_by_type("document.pdf", "filtered_images", ['.jpg', '.jpeg', '.png'])
```

{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Saves only images matching the specified formats, skipping others.

## Batch save images from multiple documents

To extract and save images from multiple documents:
{{< tabs "example-7" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from pathlib import Path
import os

def batch_save_images(input_dir, output_dir):
    """
    Extract and save images from all documents in a directory.
    """
    os.makedirs(output_dir, exist_ok=True)
    
    extensions = ['.pdf', '.docx', '.doc', '.pptx', '.ppt']
    
    for file_path in Path(input_dir).rglob('*'):
        if file_path.suffix.lower() in extensions:
            print(f"
Processing: {file_path.name}")
            
            # Create subdirectory for this document
            doc_output_dir = os.path.join(output_dir, file_path.stem)
            os.makedirs(doc_output_dir, exist_ok=True)
            
            try:
                with Parser(str(file_path)) as parser:
                    images = parser.get_images()
                    
                    if images:
                        image_count = 0
                        for idx, image in enumerate(images):
                            filename = f"image_{idx + 1}{image.file_type.extension}"
                            filepath = os.path.join(doc_output_dir, filename)
                            image.save(filepath)
                            image_count += 1
                        
                        print(f"  Saved {image_count} images")
                    else:
                        print(f"  No images or not supported")
            
            except Exception as e:
                print(f"  Error: {e}")

# Usage
batch_save_images("input_documents", "all_extracted_images")
```

{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Processes multiple documents and saves their images in organized subdirectories.

## Notes

- The `save()` method saves images to the specified file path
- Use `ImageOptions` to convert images to different formats
- Supported output formats: BMP, GIF, JPEG, PNG, WebP
- Original image format is preserved unless `ImageOptions` is specified
- Always create the output directory before saving
- File paths can be absolute or relative
- Consider disk space when extracting many images

## Related pages

- [Extract images from document]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-images/extract-images-from-document" >}})
- [Extract images from document page]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page" >}})
- [Extract images from document page area]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page-area" >}})

