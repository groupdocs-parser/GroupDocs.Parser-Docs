---
id: extract-images-from-document-page
url: parser/python-net/extract-images-from-document-page
title: Extract images from document page
weight: 2
description: "Learn how to extract images from specific document pages using GroupDocs.Parser for Python via .NET. Page-by-page image extraction from PDF, Word, PowerPoint."
keywords: extract images from document page, extract images from page, page image extraction, extract images by page
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser allows you to extract images from specific pages of a document, providing fine-grained control over image extraction.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample documents with multiple pages
- Basic understanding of page indexing (zero-based)

## Extract images from a specific page

To extract images from a particular page:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Check if image extraction is supported
    if not parser.features.images:
        print("Document doesn't support image extraction")
        return
    
    # Extract images from page 0 (first page)
    page_index = 0
    images = parser.get_images(page_index)
    
    if images:
        print(f"Images on page {page_index + 1}:")
        for idx, image in enumerate(images):
            print(f"  Image {idx + 1}: {image.file_type}, size: {image.rectangle.width}x{image.rectangle.height}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns only the images from the specified page, or an empty collection if the page has no images.

## Extract images from all pages

To process images page by page:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.pptx") as parser:
    # Check if image extraction is supported
    if not parser.features.images:
        print("Document doesn't support image extraction")
        return
    
    # Get document info
    info = parser.get_document_info()
    
    # Check if document has pages
    if info.page_count == 0:
        print("Document has no pages")
        return
    
    # Iterate over pages
    total_images = 0
    for page_index in range(info.page_count):
        print(f"
Page {page_index + 1}/{info.page_count}:")
        
        # Extract images from the current page
        images = parser.get_images(page_index)
        
        if images:
            page_image_count = 0
            for image in images:
                print(f"  Type: {image.file_type}, Position: ({image.rectangle.left}, {image.rectangle.top})")
                page_image_count += 1
            
            print(f"  Total images on this page: {page_image_count}")
            total_images += page_image_count
        else:
            print("  No images on this page")
    
    print(f"
Total images in document: {total_images}")
```

{{< /tab >}}
{{< tab "sample.pptx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pptx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page/sample.pptx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Iterates through all pages and extracts images from each page individually, showing page-specific image counts.

## Save images by page

To save images organized by page number:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import os

# Create output directory
output_dir = "images_by_page"
os.makedirs(output_dir, exist_ok=True)

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    if not parser.features.images:
        print("Image extraction not supported")
        return
    
    # Get document info
    info = parser.get_document_info()
    
    # Process each page
    for page_index in range(info.page_count):
        # Create page-specific directory
        page_dir = os.path.join(output_dir, f"page_{page_index + 1}")
        os.makedirs(page_dir, exist_ok=True)
        
        # Extract images from page
        images = parser.get_images(page_index)
        
        if images:
            for img_idx, image in enumerate(images):
                # Generate filename
                filename = f"image_{img_idx + 1}{image.file_type.extension}"
                filepath = os.path.join(page_dir, filename)
                
                # Save image
                image.save(filepath)
                print(f"Saved: {filepath}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Creates separate directories for each page and saves extracted images with organized naming.

## Extract images from specific pages only

To extract images from selected pages:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import os

# Create an instance of Parser class
with Parser("./sample.docx") as parser:
    if not parser.features.images:
        print("Image extraction not supported")
        return
    
    # Define pages to process (e.g., pages 1, 3, and 5)
    pages_to_process = [0, 2, 4]  # Zero-based indices
    
    output_dir = "selected_page_images"
    os.makedirs(output_dir, exist_ok=True)
    
    for page_index in pages_to_process:
        print(f"
Processing page {page_index + 1}...")
        
        # Extract images from this page
        images = parser.get_images(page_index)
        
        if images:
            for img_idx, image in enumerate(images):
                filename = f"page{page_index + 1}_img{img_idx + 1}{image.file_type.extension}"
                filepath = os.path.join(output_dir, filename)
                image.save(filepath)
                print(f"  Saved: {filename}")
        else:
            print(f"  No images found on page {page_index + 1}")
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts images only from the specified pages, skipping others.

## Count images per page

To analyze image distribution across pages:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def analyze_image_distribution(file_path):
    """
    Analyze how images are distributed across document pages.
    """
    with Parser(file_path) as parser:
        if not parser.features.images:
            print("Image extraction not supported")
            return
        
        info = parser.get_document_info()
        
        image_stats = []
        total_images = 0
        
        for page_index in range(info.page_count):
            images = parser.get_images(page_index)
            
            if images:
                # Count images on this page
                image_list = list(images)
                page_image_count = len(image_list)
                
                # Collect image types
                image_types = [img.file_type.extension for img in image_list]
                
                image_stats.append({
                    'page': page_index + 1,
                    'count': page_image_count,
                    'types': image_types
                })
                
                total_images += page_image_count
        
        # Print summary
        print(f"Document: {file_path}")
        print(f"Total pages: {info.page_count}")
        print(f"Total images: {total_images}")
        print(f"Average images per page: {total_images / info.page_count:.2f}
")
        
        # Print per-page details
        for stat in image_stats:
            types_str = ", ".join(set(stat['types']))
            print(f"Page {stat['page']}: {stat['count']} images ({types_str})")

# Usage
analyze_image_distribution("sample.pdf")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Extract first image from each page

To extract only the first image from each page:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import os

# Create output directory
output_dir = "first_images"
os.makedirs(output_dir, exist_ok=True)

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    if not parser.features.images:
        print("Image extraction not supported")
        return
    
    info = parser.get_document_info()
    
    for page_index in range(info.page_count):
        # Extract images from page
        images = parser.get_images(page_index)
        
        if images:
            # Get first image only
            first_image = next(iter(images), None)
            
            if first_image:
                filename = f"page_{page_index + 1}_first{first_image.file_type.extension}"
                filepath = os.path.join(output_dir, filename)
                first_image.save(filepath)
                print(f"Saved first image from page {page_index + 1}: {filename}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts and saves only the first image found on each page.

## Notes

- Page indices are zero-based (first page is index 0)
- Use `get_document_info()` to determine the total number of pages
- Check `parser.features.images` before extracting images
- Empty collections are returned for pages without images (not `None`)
- Page-by-page extraction is memory-efficient for large documents
- The method returns `None` only if image extraction is not supported for the document format

## Related pages

- [Extract images from document]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-images/extract-images-from-document" >}})
- [Extract images from document page area]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page-area" >}})
- [Get document info]({{< ref "parser/python-net/developer-guide/basic-usage/get-document-info" >}})

