---
id: extract-hyperlinks-from-document-page
url: parser/python-net/extract-hyperlinks-from-document-page
title: Extract hyperlinks from document page
weight: 2
description: "Learn how to extract hyperlinks from specific document pages using GroupDocs.Parser for Python via .NET."
keywords: extract hyperlinks from page, page hyperlinks, extract links from specific page
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser allows extracting hyperlinks from specific pages of a document, providing page-by-page hyperlink processing.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample documents with hyperlinks
- Understanding of page indexing (zero-based)

## Extract hyperlinks from a specific page

To extract hyperlinks from a particular page:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./document.pdf") as parser:
    # Check if hyperlink extraction is supported
    if not parser.features.hyperlinks:
        print("Hyperlink extraction not supported")
        return
    
    # Extract hyperlinks from page 0 (first page)
    page_index = 0
    hyperlinks = parser.get_hyperlinks(page_index)
    
    if hyperlinks:
        print(f"Hyperlinks on page {page_index + 1}:")
        for link in hyperlinks:
            print(f"  Text: {link.text}")
            print(f"  URL: {link.url}")
            print()
```

{{< /tab >}}
{{< tab "document.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page/document.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns only hyperlinks from the specified page.

## Extract hyperlinks from all pages

To process hyperlinks page by page:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("report.docx") as parser:
    if not parser.features.hyperlinks:
        print("Hyperlink extraction not supported")
        return
    
    # Get document info
    info = parser.get_document_info()
    
    if info.page_count == 0:
        print("Document has no pages")
        return
    
    # Iterate over pages
    total_links = 0
    
    for page_index in range(info.page_count):
        print(f"
=== Page {page_index + 1}/{info.page_count} ===")
        
        # Extract hyperlinks from current page
        hyperlinks = parser.get_hyperlinks(page_index)
        
        if hyperlinks:
            page_links = list(hyperlinks)
            print(f"Found {len(page_links)} hyperlinks:")
            
            for link in page_links:
                print(f"  • {link.text} -> {link.url}")
            
            total_links += len(page_links)
        else:
            print("No hyperlinks on this page")
    
    print(f"
Total hyperlinks: {total_links}")
```

{{< /tab >}}
{{< tab "report.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [report.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page/report.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Iterates through all pages and extracts hyperlinks from each page individually.

## Export page hyperlinks to JSON

To export hyperlinks organized by page:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import json

def export_page_hyperlinks(file_path, output_json):
    """
    Extract hyperlinks by page and export to JSON.
    """
    with Parser(file_path) as parser:
        if not parser.features.hyperlinks:
            print("Hyperlink extraction not supported")
            return False
        
        info = parser.get_document_info()
        
        # Build structure
        document_links = {
            'file': file_path,
            'total_pages': info.page_count,
            'pages': []
        }
        
        for page_index in range(info.page_count):
            hyperlinks = parser.get_hyperlinks(page_index)
            
            page_data = {
                'page_number': page_index + 1,
                'hyperlinks': []
            }
            
            if hyperlinks:
                for link in hyperlinks:
                    page_data['hyperlinks'].append({
                        'text': link.text or '',
                        'url': link.url or ''
                    })
            
            document_links['pages'].append(page_data)
        
        # Save to JSON
        with open(output_json, 'w', encoding='utf-8') as f:
            json.dump(document_links, f, indent=2, ensure_ascii=False)
        
        print(f"Hyperlinks exported to {output_json}")
        return True

# Usage
export_page_hyperlinks("document.pdf", "hyperlinks_by_page.json")
```

{{< /tab >}}
{{< tab "document.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page/document.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "hyperlinks_by_page.json" >}}
{{< tab-text >}}
The following sample file is used in this example: [hyperlinks_by_page.json](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page/hyperlinks_by_page.json)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Creates a JSON file with hyperlinks organized by page number.

## Find pages with external links

To identify pages containing external links:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def find_pages_with_external_links(file_path):
    """
    Find all pages containing external (http/https) links.
    """
    with Parser(file_path) as parser:
        if not parser.features.hyperlinks:
            print("Hyperlink extraction not supported")
            return []
        
        info = parser.get_document_info()
        pages_with_external_links = []
        
        for page_index in range(info.page_count):
            hyperlinks = parser.get_hyperlinks(page_index)
            
            if hyperlinks:
                has_external = False
                external_links = []
                
                for link in hyperlinks:
                    if link.url and (link.url.startswith('http://') or link.url.startswith('https://')):
                        has_external = True
                        external_links.append(link.url)
                
                if has_external:
                    pages_with_external_links.append({
                        'page': page_index + 1,
                        'links': external_links
                    })
        
        return pages_with_external_links

# Usage
pages = find_pages_with_external_links("article.pdf")

print(f"Pages with external links: {len(pages)}
")
for page_info in pages:
    print(f"Page {page_info['page']}: {len(page_info['links'])} external links")
    for url in page_info['links'][:3]:  # Show first 3
        print(f"  - {url}")
```

{{< /tab >}}
{{< tab "article.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [article.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page/article.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns a list of pages containing external hyperlinks.

## Compare hyperlinks across pages

To analyze hyperlink distribution:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from collections import defaultdict

def analyze_hyperlink_distribution(file_path):
    """
    Analyze how hyperlinks are distributed across pages.
    """
    with Parser(file_path) as parser:
        if not parser.features.hyperlinks:
            print("Hyperlink extraction not supported")
            return
        
        info = parser.get_document_info()
        
        # Collect statistics
        stats = []
        domain_frequency = defaultdict(int)
        
        for page_index in range(info.page_count):
            hyperlinks = parser.get_hyperlinks(page_index)
            
            if hyperlinks:
                link_list = list(hyperlinks)
                page_stat = {
                    'page': page_index + 1,
                    'count': len(link_list),
                    'unique_urls': len(set(link.url for link in link_list))
                }
                stats.append(page_stat)
                
                # Count domains
                for link in link_list:
                    if link.url:
                        try:
                            from urllib.parse import urlparse
                            domain = urlparse(link.url).netloc or "internal"
                            domain_frequency[domain] += 1
                        except:
                            pass
        
        # Print analysis
        print(f"Hyperlink Distribution Analysis:")
        print(f"{'Page':<10}{'Links':<10}{'Unique URLs':<15}")
        print("-" * 35)
        
        for stat in stats:
            print(f"{stat['page']:<10}{stat['count']:<10}{stat['unique_urls']:<15}")
        
        print(f"
Top domains:")
        for domain, count in sorted(domain_frequency.items(), key=lambda x: x[1], reverse=True)[:5]:
            print(f"  {domain}: {count}")

# Usage
analyze_hyperlink_distribution("documentation.pdf")
```

{{< /tab >}}
{{< tab "documentation.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [documentation.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page/documentation.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Provides statistical analysis of hyperlink distribution across pages.

## Extract hyperlinks from page range

To extract hyperlinks from a range of pages:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def extract_hyperlinks_from_range(file_path, start_page, end_page):
    """
    Extract hyperlinks from a range of pages.
    
    Args:
        file_path: Path to document
        start_page: Starting page number (1-based)
        end_page: Ending page number (1-based, inclusive)
    """
    with Parser(file_path) as parser:
        if not parser.features.hyperlinks:
            print("Hyperlink extraction not supported")
            return []
        
        info = parser.get_document_info()
        
        # Validate range
        start_idx = max(0, start_page - 1)
        end_idx = min(info.page_count - 1, end_page - 1)
        
        all_links = []
        
        for page_index in range(start_idx, end_idx + 1):
            hyperlinks = parser.get_hyperlinks(page_index)
            
            if hyperlinks:
                for link in hyperlinks:
                    all_links.append({
                        'page': page_index + 1,
                        'text': link.text,
                        'url': link.url
                    })
        
        return all_links

# Usage - extract hyperlinks from pages 1-5
links = extract_hyperlinks_from_range("document.pdf", 1, 5)
print(f"Found {len(links)} hyperlinks in pages 1-5
")

for link in links:
    print(f"Page {link['page']}: {link['text']} -> {link['url']}")
```

{{< /tab >}}
{{< tab "document.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page/document.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts hyperlinks only from the specified page range.

## Notes

- Page indices are zero-based (first page is index 0)
- Use `get_document_info()` to determine total page count
- Check `parser.features.hyperlinks` before extraction
- Empty collections are returned for pages without hyperlinks (not `None`)
- Page-by-page extraction is memory-efficient for large documents

## Related pages

- [Extract hyperlinks from document]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document" >}})
- [Extract hyperlinks from document page area]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document-page-area" >}})
- [Get document info]({{< ref "parser/python-net/developer-guide/basic-usage/get-document-info" >}})

