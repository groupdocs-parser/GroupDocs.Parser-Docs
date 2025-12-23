---
id: extract-text-areas
url: parser/python-net/extract-text-areas
title: Extract text areas
weight: 7
description: "Learn how to extract text areas with coordinates and formatting information from documents using GroupDocs.Parser for Python via .NET. Extract text with position data, rectangles, and text styles."
keywords: extract text areas, extract text areas from documents, extract text with coordinates, text position extraction, text area extraction
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser provides functionality to extract text areas with position information (coordinates) and formatting details from documents.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample documents for testing
- Understanding of coordinate systems in documents

## What are text areas?

Text areas represent rectangular regions on a page containing text. Each text area includes:

- **Page index:** The page where the text appears
- **Rectangle:** Position and size (x, y, width, height)
- **Text:** The actual text content
- **Baseline:** The baseline position of the text
- **Text style:** Formatting information (font, size, etc.)
- **Child areas:** For composite text areas

## Extract text areas from document

To extract all text areas from a document:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Extract text areas
    text_areas = parser.get_text_areas()
    
    # Check if text areas extraction is supported
    if text_areas is None:
        print("Text areas extraction isn't supported")
    else:
        # Iterate over text areas
        for area in text_areas:
            # Print page index, rectangle, and text
            print(f"Page: {area.page.index}, Rectangle: {area.rectangle}, Text: {area.text}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-areas/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns a collection of `PageTextArea` objects with position and text information for each text area in the document.

## Extract text areas from document page

To extract text areas from a specific page:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Check if text areas extraction is supported
    if not parser.features.text_areas:
        print("Document doesn't support text areas extraction")
        return
    
    # Get document info
    info = parser.get_document_info()
    
    # Check if document has pages
    if info.page_count == 0:
        print("Document has no pages")
        return
    
    # Iterate over pages
    for page_index in range(info.page_count):
        print(f"
Page {page_index + 1}/{info.page_count}")
        
        # Extract text areas from the page
        text_areas = parser.get_text_areas(page_index)
        
        # Iterate over text areas
        if text_areas:
            for area in text_areas:
                # Print rectangle and text
                rect = area.rectangle
                print(f"Position: ({rect.left}, {rect.top}), Size: ({rect.width}x{rect.height})")
                print(f"Text: {area.text}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-areas/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns text areas only for the specified page, allowing page-by-page processing.

## Extract text areas with options

To extract text areas from a specific region with filtering:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageTextAreaOptions, Rectangle, Point, Size

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Define the region (upper-left corner, 300x100 pixels)
    region = Rectangle(Point(0, 0), Size(300, 100))
    
    # Create options to extract only text areas with digits in the specified region
    options = PageTextAreaOptions(r"\d+", region)
    
    # Extract text areas
    text_areas = parser.get_text_areas(options)
    
    if text_areas is None:
        print("Text areas extraction isn't supported")
    else:
        # Iterate over filtered text areas
        for area in text_areas:
            print(f"Page: {area.page.index}, Rectangle: {area.rectangle}, Text: {area.text}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-areas/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns only text areas that match the regular expression pattern and fall within the specified rectangular region.

## Extract text areas with formatting

To extract text areas and access formatting information:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.docx") as parser:
    # Extract text areas
    text_areas = parser.get_text_areas()
    
    if text_areas is None:
        print("Text areas extraction isn't supported")
    else:
        # Iterate over text areas
        for area in text_areas:
            # Get text style if available
            if area.text_style:
                print(f"Text: {area.text}")
                print(f"Font: {area.text_style.name}")
                print(f"Size: {area.text_style.font_size}")
                print(f"Bold: {area.text_style.is_bold}")
                print(f"Italic: {area.text_style.is_italic}")
                print("---")
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-areas/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts text areas with detailed formatting information, useful for document analysis.

## Extract specific text from regions

Extract text from multiple predefined regions:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageTextAreaOptions, Rectangle, Point, Size

def extract_from_regions(file_path, regions):
    """
    Extract text from specific regions of a document.
    
    Args:
        file_path: Path to the document
        regions: List of tuples (x, y, width, height)
    """
    with Parser(file_path) as parser:
        if not parser.features.text_areas:
            print("Text areas extraction not supported")
            return {}
        
        results = {}
        
        for idx, (x, y, w, h) in enumerate(regions):
            # Create rectangle for this region
            rect = Rectangle(Point(x, y), Size(w, h))
            options = PageTextAreaOptions(None, rect)
            
            # Extract text areas from this region
            areas = parser.get_text_areas(options)
            
            # Collect text from all areas in this region
            text_list = [area.text for area in areas] if areas else []
            results[f"region_{idx}"] = " ".join(text_list)
        
        return results

# Define regions (e.g., header, body, footer)
regions = [
    (0, 0, 600, 100),      # Header region
    (0, 100, 600, 700),    # Body region
    (0, 800, 600, 100)     # Footer region
]

# Extract text from regions
extracted = extract_from_regions("sample.pdf", regions)
for region_name, text in extracted.items():
    print(f"{region_name}: {text[:100]}...")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-areas/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Notes

- Text area extraction is more detailed than simple text extraction
- Not all document formats support text area extraction - check `parser.features.text_areas` first
- Coordinates are in document-specific units (usually points or pixels)
- Composite text areas contain child text areas in the `areas` property
- Use regular expressions in `PageTextAreaOptions` to filter text areas by content
- Rectangle coordinates start from the top-left corner (0, 0)

## Related pages

- [Extract text in Accurate mode]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-in-accurate-mode" >}})
- [Search text in documents]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/search-text" >}})
- [Extract text structure]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-structure" >}})

