---
id: extract-images-from-document-page-area
url: parser/python-net/extract-images-from-document-page-area
title: Extract images from document page area
weight: 3
description: "Learn how to extract images from specific page areas using GroupDocs.Parser for Python via .NET. Extract images from defined rectangular regions."
keywords: extract images from document page area, extract images from region, area image extraction
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser allows extracting images from specific rectangular areas of a document page, enabling precise image extraction from defined regions.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample documents with images
- Understanding of coordinate systems

## Extract images from page area

To extract images from a specific rectangular area:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageAreaOptions, Rectangle, Point, Size

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Check if image extraction is supported
    if not parser.features.images:
        print("Image extraction not supported")
        return
    
    # Define the area (upper-left corner, 300x100 pixels)
    area = Rectangle(Point(0, 0), Size(300, 100))
    
    # Create options for the area
    options = PageAreaOptions(area)
    
    # Extract images from the specified area
    images = parser.get_images(options)
    
    if images:
        print(f"Images found in area:")
        for idx, image in enumerate(images):
            print(f"  Image {idx + 1}: {image.file_type}, Page {image.page.index + 1}")
    else:
        print("No images found in the specified area")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page-area/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns only images that are located within the specified rectangular area.

## Extract images from multiple regions

To extract images from several predefined areas:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageAreaOptions, Rectangle, Point, Size
import os

def extract_images_from_regions(file_path, regions, output_dir):
    """
    Extract images from multiple predefined regions.
    
    Args:
        file_path: Path to document
        regions: List of tuples (x, y, width, height)
        output_dir: Directory to save images
    """
    os.makedirs(output_dir, exist_ok=True)
    
    with Parser(file_path) as parser:
        if not parser.features.images:
            print("Image extraction not supported")
            return
        
        for region_idx, (x, y, w, h) in enumerate(regions):
            print(f"
Processing region {region_idx + 1}: ({x}, {y}, {w}x{h})")
            
            # Create rectangle for this region
            area = Rectangle(Point(x, y), Size(w, h))
            options = PageAreaOptions(area)
            
            # Extract images from this region
            images = parser.get_images(options)
            
            if images:
                for img_idx, image in enumerate(images):
                    filename = f"region{region_idx + 1}_img{img_idx + 1}{image.file_type.extension}"
                    filepath = os.path.join(output_dir, filename)
                    image.save(filepath)
                    print(f"  Saved: {filename}")

# Define regions (e.g., header, body, footer)
regions = [
    (0, 0, 600, 100),      # Header region
    (0, 100, 600, 700),    # Body region
    (0, 800, 600, 100)     # Footer region
]

# Extract images from regions
extract_images_from_regions("document.pdf", regions, "region_images")
```

{{< /tab >}}
{{< tab "document.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page-area/document.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts and saves images organized by the region they were found in.

## Extract images from page area with page index

To extract images from a specific area on a specific page:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageAreaOptions, Rectangle, Point, Size

# Create an instance of Parser class
with Parser("document.pptx") as parser:
    if not parser.features.images:
        print("Image extraction not supported")
        return
    
    # Get document info
    info = parser.get_document_info()
    
    # Process specific page (e.g., first page)
    page_index = 0
    
    if page_index < info.page_count:
        # Define area (center of page, 400x300)
        area = Rectangle(Point(100, 100), Size(400, 300))
        options = PageAreaOptions(area)
        
        # Extract images from the area on the specific page
        images = parser.get_images(page_index, options)
        
        if images:
            print(f"Images on page {page_index + 1} in specified area:")
            for image in images:
                print(f"  Type: {image.file_type}")
                print(f"  Position: ({image.rectangle.left}, {image.rectangle.top})")
                print(f"  Size: {image.rectangle.width}x{image.rectangle.height}")
```

{{< /tab >}}
{{< tab "document.pptx" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.pptx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page-area/document.pptx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts images from a specific area on a specific page.

## Extract logo from document header

To extract logo images from the header area:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageAreaOptions, Rectangle, Point, Size
import os

def extract_header_logos(file_path, output_dir):
    """
    Extract logo images from document header area.
    """
    os.makedirs(output_dir, exist_ok=True)
    
    with Parser(file_path) as parser:
        if not parser.features.images:
            print("Image extraction not supported")
            return
        
        # Define header area (top 150 pixels, full width)
        header_area = Rectangle(Point(0, 0), Size(1000, 150))
        options = PageAreaOptions(header_area)
        
        # Extract images from header
        images = parser.get_images(options)
        
        if images:
            print(f"Found {len(list(images))} images in header
")
            
            # Re-extract as iterator was consumed
            images = parser.get_images(options)
            
            for idx, image in enumerate(images):
                filename = f"logo_{idx + 1}{image.file_type.extension}"
                filepath = os.path.join(output_dir, filename)
                image.save(filepath)
                print(f"Saved logo: {filename}")
        else:
            print("No images found in header area")

# Usage
extract_header_logos("invoice.pdf", "extracted_logos")
```

{{< /tab >}}
{{< tab "invoice.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [invoice.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page-area/invoice.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts logo or brand images typically found in document headers.

## Extract images from specific quadrants

To extract images from document quadrants:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageAreaOptions, Rectangle, Point, Size

def extract_by_quadrant(file_path, page_width=600, page_height=800):
    """
    Extract images by document quadrant (TL, TR, BL, BR).
    """
    # Define quadrants
    quadrant_size_w = page_width // 2
    quadrant_size_h = page_height // 2
    
    quadrants = {
        'Top-Left': Rectangle(Point(0, 0), Size(quadrant_size_w, quadrant_size_h)),
        'Top-Right': Rectangle(Point(quadrant_size_w, 0), Size(quadrant_size_w, quadrant_size_h)),
        'Bottom-Left': Rectangle(Point(0, quadrant_size_h), Size(quadrant_size_w, quadrant_size_h)),
        'Bottom-Right': Rectangle(Point(quadrant_size_w, quadrant_size_h), Size(quadrant_size_w, quadrant_size_h))
    }
    
    with Parser(file_path) as parser:
        if not parser.features.images:
            print("Image extraction not supported")
            return
        
        for quadrant_name, area in quadrants.items():
            options = PageAreaOptions(area)
            images = parser.get_images(options)
            
            if images:
                image_list = list(images)
                print(f"{quadrant_name}: {len(image_list)} images")
            else:
                print(f"{quadrant_name}: 0 images")

# Usage
extract_by_quadrant("layout.pdf")
```

{{< /tab >}}
{{< tab "layout.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [layout.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page-area/layout.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Categorizes and counts images by their location in document quadrants.

## Extract images larger than specific size

To filter images by size within an area:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageAreaOptions, Rectangle, Point, Size
import os

def extract_large_images_from_area(file_path, area_rect, min_width, min_height, output_dir):
    """
    Extract images larger than specified size from an area.
    """
    os.makedirs(output_dir, exist_ok=True)
    
    with Parser(file_path) as parser:
        if not parser.features.images:
            print("Image extraction not supported")
            return
        
        options = PageAreaOptions(area_rect)
        images = parser.get_images(options)
        
        if not images:
            print("No images found in area")
            return
        
        saved_count = 0
        
        for image in images:
            # Check image size
            if image.rectangle.width >= min_width and image.rectangle.height >= min_height:
                filename = f"large_image_{saved_count + 1}{image.file_type.extension}"
                filepath = os.path.join(output_dir, filename)
                image.save(filepath)
                
                print(f"Saved: {filename} ({image.rectangle.width}x{image.rectangle.height})")
                saved_count += 1
        
        print(f"
Total large images saved: {saved_count}")

# Usage - extract images larger than 100x100 from center area
center_area = Rectangle(Point(100, 100), Size(400, 600))
extract_large_images_from_area("document.pdf", center_area, 100, 100, "large_images")
```

{{< /tab >}}
{{< tab "document.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page-area/document.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts only images that meet the minimum size criteria within the specified area.

## Notes

- Always check `parser.features.images` before extracting images
- Coordinates are in document-specific units (points or pixels)
- The origin (0, 0) is at the top-left corner
- Images partially overlapping the area boundary are included
- Use `get_images(page_index, options)` to extract from a specific page area
- Empty collections are returned if no images are found in the area (not `None`)

## Related pages

- [Extract images from document]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-images/extract-images-from-document" >}})
- [Extract images from document page]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-images/extract-images-from-document-page" >}})
- [Extract images to files]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-images/extract-images-to-files" >}})

