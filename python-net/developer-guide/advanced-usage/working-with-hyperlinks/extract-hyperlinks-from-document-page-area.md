---
id: extract-hyperlinks-from-document-page-area
url: parser/python-net/extract-hyperlinks-from-document-page-area
title: Extract hyperlinks from document page area
weight: 3
description: "Learn how to extract hyperlinks from specific page areas using GroupDocs.Parser for Python via .NET. Extract links from defined rectangular regions."
keywords: extract hyperlinks from area, area hyperlinks, extract links from region
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser allows extracting hyperlinks from specific rectangular areas of document pages, enabling precise link extraction from defined regions.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample documents with hyperlinks
- Understanding of coordinate systems

## Extract hyperlinks from page area

To extract hyperlinks from a specific rectangular area:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageAreaOptions, Rectangle, Point, Size

# Create an instance of Parser class
with Parser("./webpage.html") as parser:
    # Check if hyperlink extraction is supported
    if not parser.features.hyperlinks:
        print("Hyperlink extraction not supported")
        sys.exit()
    
    # Define the area (upper portion, 600x200 pixels)
    area = Rectangle(Point(0, 0), Size(600, 200))
    
    # Create options for the area
    options = PageAreaOptions(area)
    
    # Extract hyperlinks from the specified area
    hyperlinks = parser.get_hyperlinks(options)
    
    if hyperlinks:
        print(f"Hyperlinks found in area:")
        for link in hyperlinks:
            print(f"  {link.text} -> {link.url}")
    else:
        print("No hyperlinks found in the specified area")
```

{{< /tab >}}
{{< tab "webpage.htm" >}}
{{< tab-text >}}
The following sample file is used in this example: [webpage.htm](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page-area/webpage.htm)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns only hyperlinks located within the specified rectangular area.

## Extract hyperlinks from specific page area

To extract hyperlinks from an area on a specific page:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageAreaOptions, Rectangle, Point, Size

# Create an instance of Parser class
with Parser("document.pdf") as parser:
    if not parser.features.hyperlinks:
        print("Hyperlink extraction not supported")
        sys.exit()
    
    # Define area (sidebar region)
    area = Rectangle(Point(400, 0), Size(200, 800))
    options = PageAreaOptions(area)
    
    # Get document info
    info = parser.get_document_info()
    
    # Process first page
    page_index = 0
    
    if page_index < info.page_count:
        # Extract hyperlinks from area on specific page
        hyperlinks = parser.get_hyperlinks(page_index, options)
        
        if hyperlinks:
            print(f"Hyperlinks in sidebar (page {page_index + 1}):")
            for link in hyperlinks:
                print(f"  {link.text}: {link.url}")
```

{{< /tab >}}
{{< tab "document.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page-area/document.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts hyperlinks from a specific area on a specific page.

## Extract navigation links from header

To extract navigation links from the header area:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageAreaOptions, Rectangle, Point, Size

def extract_header_links(file_path):
    """
    Extract navigation links from document header.
    """
    with Parser(file_path) as parser:
        if not parser.features.hyperlinks:
            print("Hyperlink extraction not supported")
            return []
        
        # Define header area (top 100 pixels, full width)
        header_area = Rectangle(Point(0, 0), Size(1000, 100))
        options = PageAreaOptions(header_area)
        
        # Extract hyperlinks from header
        hyperlinks = parser.get_hyperlinks(options)
        
        if hyperlinks:
            links = []
            for link in hyperlinks:
                links.append({
                    'text': link.text or 'No text',
                    'url': link.url or ''
                })
            return links
        
        return []

# Usage
nav_links = extract_header_links("website.html")

print(f"Navigation links ({len(nav_links)}):")
for link in nav_links:
    print(f"  {link['text']} → {link['url']}")
```

{{< /tab >}}
{{< tab "website.htm" >}}
{{< tab-text >}}
The following sample file is used in this example: [website.htm](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page-area/website.htm)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts navigation or menu links typically found in document headers.

## Extract links from multiple regions

To extract hyperlinks from several predefined areas:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageAreaOptions, Rectangle, Point, Size

def extract_links_from_regions(file_path, regions):
    """
    Extract hyperlinks from multiple regions.
    
    Args:
        file_path: Path to document
        regions: Dict of region names to (x, y, width, height) tuples
    """
    with Parser(file_path) as parser:
        if not parser.features.hyperlinks:
            print("Hyperlink extraction not supported")
            return {}
        
        results = {}
        
        for region_name, (x, y, w, h) in regions.items():
            # Create rectangle for this region
            area = Rectangle(Point(x, y), Size(w, h))
            options = PageAreaOptions(area)
            
            # Extract hyperlinks from this region
            hyperlinks = parser.get_hyperlinks(options)
            
            if hyperlinks:
                results[region_name] = [link.url for link in hyperlinks]
            else:
                results[region_name] = []
        
        return results

# Define regions
regions = {
    'header': (0, 0, 600, 100),
    'sidebar': (0, 100, 150, 700),
    'footer': (0, 800, 600, 100)
}

# Extract links from regions
links_by_region = extract_links_from_regions("page.html", regions)

for region, urls in links_by_region.items():
    print(f"{region.upper()}: {len(urls)} links")
    for url in urls[:5]:  # Show first 5
        print(f"  - {url}")
```

{{< /tab >}}
{{< tab "page.htm" >}}
{{< tab-text >}}
The following sample file is used in this example: [page.htm](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page-area/page.htm)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts and categorizes hyperlinks by document region.

## Extract footer links

To extract links from the footer area:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageAreaOptions, Rectangle, Point, Size

def extract_footer_links(file_path, page_height=842):
    """
    Extract links from document footer.
    
    Args:
        file_path: Path to document
        page_height: Approximate page height in points (default: A4 = 842)
    """
    with Parser(file_path) as parser:
        if not parser.features.hyperlinks:
            print("Hyperlink extraction not supported")
            return []
        
        # Define footer area (bottom 100 pixels)
        footer_y = page_height - 100
        footer_area = Rectangle(Point(0, footer_y), Size(600, 100))
        options = PageAreaOptions(footer_area)
        
        # Extract hyperlinks from footer
        hyperlinks = parser.get_hyperlinks(options)
        
        if hyperlinks:
            links = []
            for link in hyperlinks:
                links.append({
                    'text': link.text or '',
                    'url': link.url or '',
                    'position': (link.rectangle.left, link.rectangle.top)
                })
            return links
        
        return []

# Usage
footer_links = extract_footer_links("document.pdf")

print(f"Footer links: {len(footer_links)}")
for link in footer_links:
    print(f"{link['text']}: {link['url']}")
```

{{< /tab >}}
{{< tab "document.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page-area/document.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts links commonly found in document footers (e.g., copyright, contact, social media).

## Filter links by area and URL pattern

To extract links from an area matching a URL pattern:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageAreaOptions, Rectangle, Point, Size
import re

def extract_links_by_area_and_pattern(file_path, area_rect, url_pattern):
    """
    Extract links from area matching URL pattern.
    
    Args:
        file_path: Path to document
        area_rect: Tuple (x, y, width, height)
        url_pattern: Regex pattern for URLs
    """
    x, y, w, h = area_rect
    area = Rectangle(Point(x, y), Size(w, h))
    options = PageAreaOptions(area)
    
    with Parser(file_path) as parser:
        if not parser.features.hyperlinks:
            print("Hyperlink extraction not supported")
            return []
        
        hyperlinks = parser.get_hyperlinks(options)
        
        if not hyperlinks:
            return []
        
        # Filter by URL pattern
        matching_links = []
        pattern = re.compile(url_pattern, re.IGNORECASE)
        
        for link in hyperlinks:
            if link.url and pattern.search(link.url):
                matching_links.append({
                    'text': link.text,
                    'url': link.url
                })
        
        return matching_links

# Usage - extract PDF links from sidebar
sidebar_area = (400, 100, 200, 700)
pdf_links = extract_links_by_area_and_pattern(
    "document.html",
    sidebar_area,
    r'\.pdf$'
)

print(f"PDF download links in sidebar: {len(pdf_links)}
")
for link in pdf_links:
    print(f"  {link['text']}: {link['url']}")
```

{{< /tab >}}
{{< tab "document.htm" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.htm](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page-area/document.htm)
{{< /tab-text >}}
{{< /tab >}}
{{< tab ".pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [.pdf](../../../../../../../../../.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts only links from the specified area that match the URL pattern.

## Extract social media links from specific area

To extract social media links from a defined region:
{{< tabs "example-7" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageAreaOptions, Rectangle, Point, Size

def extract_social_media_links(file_path, area_rect):
    """
    Extract social media links from a specific area.
    """
    x, y, w, h = area_rect
    area = Rectangle(Point(x, y), Size(w, h))
    options = PageAreaOptions(area)
    
    # Social media domains
    social_domains = [
        'facebook.com', 'twitter.com', 'linkedin.com',
        'instagram.com', 'youtube.com', 'tiktok.com'
    ]
    
    with Parser(file_path) as parser:
        if not parser.features.hyperlinks:
            print("Hyperlink extraction not supported")
            return {}
        
        hyperlinks = parser.get_hyperlinks(options)
        
        if not hyperlinks:
            return {}
        
        # Categorize by platform
        social_links = {}
        
        for link in hyperlinks:
            if link.url:
                for domain in social_domains:
                    if domain in link.url:
                        platform = domain.split('.')[0].capitalize()
                        if platform not in social_links:
                            social_links[platform] = []
                        social_links[platform].append(link.url)
                        break
        
        return social_links

# Usage - extract from footer area
footer_area = (0, 800, 600, 100)
social_links = extract_social_media_links("webpage.html", footer_area)

print("Social Media Links:")
for platform, urls in social_links.items():
    print(f"  {platform}: {', '.join(urls)}")
```

{{< /tab >}}
{{< tab "webpage.htm" >}}
{{< tab-text >}}
The following sample file is used in this example: [webpage.htm](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page-area/webpage.htm)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts and categorizes social media links from a specific area.

## Notes

- Coordinates are in document-specific units (points or pixels)
- The origin (0, 0) is at the top-left corner
- Hyperlinks partially overlapping the area boundary are included
- Use `get_hyperlinks(page_index, options)` for page-specific area extraction
- Empty collections are returned if no hyperlinks are found (not `None`)
- Combine with page index for precise multi-page area extraction

## Related pages

- [Extract hyperlinks from document]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document" >}})
- [Extract hyperlinks from document page]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page" >}})
- [Extract text areas]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-areas" >}})

