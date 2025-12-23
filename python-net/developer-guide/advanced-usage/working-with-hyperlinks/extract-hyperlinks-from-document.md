---
id: extract-hyperlinks-from-document
url: parser/python-net/extract-hyperlinks-from-document
title: Extract hyperlinks from document
weight: 1
description: "Learn how to extract hyperlinks from documents using GroupDocs.Parser for Python via .NET. Extract links with text and URLs from PDF, Word, Excel."
keywords: extract hyperlinks from documents, extract hyperlinks, extract links, hyperlink extraction
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser provides functionality to extract hyperlinks from documents including their text, URL, and position information.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample documents containing hyperlinks
- Basic understanding of hyperlink structure

## Extract hyperlinks from document

To extract all hyperlinks from a document:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Check if hyperlink extraction is supported
    if not parser.features.hyperlinks:
        print("Document doesn't support hyperlink extraction")
        return
    
    # Extract hyperlinks from the document
    hyperlinks = parser.get_hyperlinks()
    
    if hyperlinks:
        # Iterate over hyperlinks
        for hyperlink in hyperlinks:
            # Print the hyperlink text
            print(f"Text: {hyperlink.text}")
            # Print the hyperlink URL
            print(f"URL: {hyperlink.url}")
            # Print page number
            print(f"Page: {hyperlink.page.index + 1}")
            print()
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns a collection of `PageHyperlinkArea` objects representing all hyperlinks found in the document, or `None` if hyperlink extraction is not supported.

## Extract hyperlinks with position information

To extract hyperlinks with detailed position data:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("document.docx") as parser:
    # Check if hyperlink extraction is supported
    if not parser.features.hyperlinks:
        print("Hyperlink extraction not supported")
        return
    
    # Extract hyperlinks
    hyperlinks = parser.get_hyperlinks()
    
    if hyperlinks:
        for idx, hyperlink in enumerate(hyperlinks):
            print(f"Hyperlink {idx + 1}:")
            print(f"  Text: {hyperlink.text}")
            print(f"  URL: {hyperlink.url}")
            print(f"  Page: {hyperlink.page.index + 1}")
            print(f"  Position: ({hyperlink.rectangle.left}, {hyperlink.rectangle.top})")
            print(f"  Size: {hyperlink.rectangle.width}x{hyperlink.rectangle.height}")
            print()
```

{{< /tab >}}
{{< tab "document.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document/document.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Displays comprehensive information about each hyperlink including text, URL, page number, and position coordinates.

## Extract and validate URLs

To extract hyperlinks and validate URL formats:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import re

def is_valid_url(url):
    """
    Basic URL validation.
    """
    url_pattern = re.compile(
        r'^(https?|ftp)://'  # Protocol
        r'([a-zA-Z0-9.-]+)'   # Domain
        r'(:[0-9]+)?'         # Optional port
        r'(/.*)?$'            # Optional path
    )
    return url_pattern.match(url) is not None

# Create an instance of Parser class
with Parser("webpage.html") as parser:
    if not parser.features.hyperlinks:
        print("Hyperlink extraction not supported")
        return
    
    # Extract hyperlinks
    hyperlinks = parser.get_hyperlinks()
    
    if hyperlinks:
        valid_links = []
        invalid_links = []
        
        for hyperlink in hyperlinks:
            if is_valid_url(hyperlink.url):
                valid_links.append(hyperlink)
            else:
                invalid_links.append(hyperlink)
        
        print(f"Valid hyperlinks: {len(valid_links)}")
        print(f"Invalid hyperlinks: {len(invalid_links)}")
        
        print("\nValid URLs:")
        for link in valid_links[:10]:  # Show first 10
            print(f"  {link.text} -> {link.url}")
```

{{< /tab >}}
{{< tab "webpage.htm" >}}
{{< tab-text >}}
The following sample file is used in this example: [webpage.htm](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document/webpage.htm)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Categorizes hyperlinks into valid and invalid based on URL format validation.

## Export hyperlinks to CSV

To export hyperlinks to a CSV file:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import csv

def export_hyperlinks_to_csv(file_path, output_csv):
    """
    Extract hyperlinks and save to CSV file.
    """
    with Parser(file_path) as parser:
        if not parser.features.hyperlinks:
            print("Hyperlink extraction not supported")
            return False
        
        hyperlinks = parser.get_hyperlinks()
        
        if not hyperlinks:
            print("No hyperlinks found")
            return False
        
        # Write to CSV
        with open(output_csv, 'w', newline='', encoding='utf-8') as csvfile:
            fieldnames = ['Text', 'URL', 'Page', 'X', 'Y', 'Width', 'Height']
            writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
            
            writer.writeheader()
            
            for hyperlink in hyperlinks:
                writer.writerow({
                    'Text': hyperlink.text or '',
                    'URL': hyperlink.url or '',
                    'Page': hyperlink.page.index + 1,
                    'X': hyperlink.rectangle.left,
                    'Y': hyperlink.rectangle.top,
                    'Width': hyperlink.rectangle.width,
                    'Height': hyperlink.rectangle.height
                })
        
        print(f"Hyperlinks exported to {output_csv}")
        return True

# Usage
export_hyperlinks_to_csv("document.pdf", "hyperlinks.csv")
```

{{< /tab >}}
{{< tab "document.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document/document.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "hyperlinks.csv" >}}
{{< tab-text >}}
The following sample file is used in this example: [hyperlinks.csv](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document/hyperlinks.csv)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Creates a CSV file containing all hyperlinks with their properties.

## Group hyperlinks by domain

To analyze hyperlink distribution by domain:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from urllib.parse import urlparse
from collections import defaultdict

def analyze_hyperlinks_by_domain(file_path):
    """
    Group hyperlinks by domain and show statistics.
    """
    with Parser(file_path) as parser:
        if not parser.features.hyperlinks:
            print("Hyperlink extraction not supported")
            return
        
        hyperlinks = parser.get_hyperlinks()
        
        if not hyperlinks:
            print("No hyperlinks found")
            return
        
        # Group by domain
        by_domain = defaultdict(list)
        
        for hyperlink in hyperlinks:
            try:
                parsed_url = urlparse(hyperlink.url)
                domain = parsed_url.netloc or "internal"
                by_domain[domain].append(hyperlink)
            except:
                by_domain["invalid"].append(hyperlink)
        
        # Print statistics
        print(f"Total hyperlinks: {sum(len(links) for links in by_domain.values())}")
        print(f"Unique domains: {len(by_domain)}
")
        
        # Sort by count
        sorted_domains = sorted(by_domain.items(), key=lambda x: len(x[1]), reverse=True)
        
        print(f"{'Domain':<40} {'Count':<10}")
        print("-" * 50)
        for domain, links in sorted_domains[:20]:  # Top 20
            print(f"{domain:<40} {len(links):<10}")

# Usage
analyze_hyperlinks_by_domain("article.html")
```

{{< /tab >}}
{{< tab "article.htm" >}}
{{< tab-text >}}
The following sample file is used in this example: [article.htm](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document/article.htm)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Provides statistical analysis of hyperlinks grouped by domain.

## Extract hyperlinks with filtering

To extract only specific types of hyperlinks:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def extract_filtered_hyperlinks(file_path, url_pattern=None, text_pattern=None):
    """
    Extract hyperlinks with optional filtering by URL or text pattern.
    """
    import re
    
    with Parser(file_path) as parser:
        if not parser.features.hyperlinks:
            print("Hyperlink extraction not supported")
            return []
        
        hyperlinks = parser.get_hyperlinks()
        
        if not hyperlinks:
            return []
        
        filtered_links = []
        
        for hyperlink in hyperlinks:
            # Apply URL pattern filter
            if url_pattern and not re.search(url_pattern, hyperlink.url, re.IGNORECASE):
                continue
            
            # Apply text pattern filter
            if text_pattern and hyperlink.text and not re.search(text_pattern, hyperlink.text, re.IGNORECASE):
                continue
            
            filtered_links.append(hyperlink)
        
        return filtered_links

# Usage examples

# Extract only PDF links
pdf_links = extract_filtered_hyperlinks("document.html", url_pattern=r'\.pdf$')
print(f"Found {len(pdf_links)} PDF links")

# Extract links containing "download"
download_links = extract_filtered_hyperlinks("document.html", text_pattern=r'download')
print(f"Found {len(download_links)} download links")

# Extract external links (http/https)
external_links = extract_filtered_hyperlinks("document.html", url_pattern=r'^https?://')
print(f"Found {len(external_links)} external links")
```

{{< /tab >}}
{{< tab "document.htm" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.htm](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document/document.htm)
{{< /tab-text >}}
{{< /tab >}}
{{< tab ".pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [.pdf](../../../../../../../../../.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns only hyperlinks matching the specified criteria.

## Batch hyperlink extraction

To extract hyperlinks from multiple documents:
{{< tabs "example-7" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from pathlib import Path
import json

def batch_extract_hyperlinks(input_dir, output_json):
    """
    Extract hyperlinks from all documents in a directory.
    """
    extensions = ['.pdf', '.docx', '.doc', '.html', '.htm']
    
    all_hyperlinks = {}
    
    for file_path in Path(input_dir).rglob('*'):
        if file_path.suffix.lower() in extensions:
            print(f"Processing: {file_path.name}")
            
            try:
                with Parser(str(file_path)) as parser:
                    if not parser.features.hyperlinks:
                        continue
                    
                    hyperlinks = parser.get_hyperlinks()
                    
                    if hyperlinks:
                        link_list = []
                        for link in hyperlinks:
                            link_list.append({
                                'text': link.text or '',
                                'url': link.url or '',
                                'page': link.page.index + 1
                            })
                        
                        all_hyperlinks[file_path.name] = {
                            'count': len(link_list),
                            'links': link_list
                        }
                
            except Exception as e:
                print(f"  Error: {e}")
    
    # Save to JSON
    with open(output_json, 'w', encoding='utf-8') as f:
        json.dump(all_hyperlinks, f, indent=2, ensure_ascii=False)
    
    total_links = sum(doc['count'] for doc in all_hyperlinks.values())
    print(f"
Total documents: {len(all_hyperlinks)}")
    print(f"Total hyperlinks: {total_links}")
    print(f"Saved to {output_json}")

# Usage
batch_extract_hyperlinks("documents", "all_hyperlinks.json")
```

{{< /tab >}}
{{< tab "all_hyperlinks.json" >}}
{{< tab-text >}}
The following sample file is used in this example: [all_hyperlinks.json](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document/all_hyperlinks.json)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Processes multiple documents and generates a comprehensive JSON report of all hyperlinks.

## Notes

- The `get_hyperlinks()` method returns `None` if hyperlink extraction is not supported
- Always check `parser.features.hyperlinks` before extracting hyperlinks
- Hyperlinks include both internal (anchors) and external (URLs) links
- The `text` property contains the visible link text
- The `url` property contains the actual hyperlink target
- Position coordinates are in document-specific units
- Some documents may have hyperlinks without visible text

## Related pages

- [Extract hyperlinks from document page]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page" >}})
- [Extract hyperlinks from document page area]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page-area" >}})
- [Extract text areas]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-areas" >}})

