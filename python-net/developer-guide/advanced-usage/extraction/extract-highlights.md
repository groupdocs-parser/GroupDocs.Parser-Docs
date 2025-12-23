---
id: extract-highlights
url: parser/python-net/extract-highlights
title: Extract highlights
weight: 5
description: "Learn how to extract text highlights (context snippets) from documents using GroupDocs.Parser for Python via .NET. Extract text with surrounding context for search results and previews."
keywords: extract highlights, text highlights, extract context, text snippets, search context, document highlights
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser provides functionality to extract highlights - text fragments with surrounding context. This feature is useful for creating search result previews, text snippets, and contextual excerpts.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample documents for testing
- Understanding of text positioning concepts

## What are highlights?

Highlights are text fragments extracted from a document at a specific position, including:
- **Text:** The main text content
- **Position:** Character position in the document
- **Context:** Surrounding text before and after

## Extract highlights

To extract a highlight from a specific position:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import HighlightOptions

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Create highlight options (extract 20 characters)
    options = HighlightOptions(20)
    
    # Extract highlight at position 100
    highlight = parser.get_highlight(100, True, options)
    
    if highlight:
        print(f"Position: {highlight.position}")
        print(f"Text: {highlight.text}")
    else:
        print("Highlight extraction not supported or position out of range")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-highlights/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns a `HighlightItem` object containing the text fragment starting at the specified position, or `None` if extraction is not supported.

## Extract highlights with fixed length

To extract highlights with a specific character count:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import HighlightOptions

# Create an instance of Parser class
with Parser("sample.docx") as parser:
    # Define positions to extract highlights from
    positions = [0, 100, 500, 1000]
    
    # Create highlight options (50 characters)
    options = HighlightOptions(50)
    
    for pos in positions:
        # Extract highlight
        highlight = parser.get_highlight(pos, True, options)
        
        if highlight:
            print(f"
Position {pos}:")
            print(f"Text: {highlight.text}")
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-highlights/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts 50-character text fragments from specified positions.

## Extract highlights for search results

To create search result previews with context:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import SearchOptions, HighlightOptions

def get_search_results_with_context(file_path, keyword, context_length=30):
    """
    Search for keyword and extract surrounding context.
    """
    with Parser(file_path) as parser:
        # Create highlight options for context
        highlight_opts = HighlightOptions(context_length)
        
        # Create search options with highlights
        search_opts = SearchOptions(
            match_case=False,
            match_whole_word=False,
            use_regular_expression=False,
            left_highlight_options=highlight_opts,
            right_highlight_options=highlight_opts
        )
        
        # Search for keyword
        results = parser.search(keyword, search_opts)
        
        if results is None:
            print("Search not supported")
            return []
        
        # Collect results with context
        search_results = []
        for result in results:
            left_context = result.left_highlight_item.text if result.left_highlight_item else ""
            right_context = result.right_highlight_item.text if result.right_highlight_item else ""
            
            search_results.append({
                'keyword': result.text,
                'position': result.position,
                'page': result.page_index + 1 if hasattr(result, 'page_index') else None,
                'left_context': left_context,
                'right_context': right_context,
                'full_snippet': f"{left_context}[{result.text}]{right_context}"
            })
        
        return search_results

# Usage
results = get_search_results_with_context("sample.pdf", "artificial intelligence", 40)

print(f"Found {len(results)} occurrences:
")
for idx, result in enumerate(results, 1):
    print(f"{idx}. {result['full_snippet']}")
    if result['page']:
        print(f"   (Page {result['page']}, position {result['position']})
")
```

{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Searches for keywords and returns results with surrounding context for better previews.

## Extract highlights from multiple positions

To extract text snippets from various document sections:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import HighlightOptions

def extract_multiple_highlights(file_path, positions, length=50):
    """
    Extract highlights from multiple positions.
    """
    highlights = []
    
    with Parser(file_path) as parser:
        options = HighlightOptions(length)
        
        for position in positions:
            highlight = parser.get_highlight(position, True, options)
            
            if highlight:
                highlights.append({
                    'position': highlight.position,
                    'text': highlight.text,
                    'length': len(highlight.text)
                })
        
        return highlights

# Extract highlights at specific positions
positions = [0, 500, 1000, 1500, 2000]
highlights = extract_multiple_highlights("sample.pdf", positions, length=80)

print(f"Extracted {len(highlights)} highlights:
")
for h in highlights:
    print(f"Position {h['position']}:")
    print(f"  {h['text'][:60]}...")
    print()
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-highlights/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts text fragments from multiple specified positions in the document.

## Create document preview with highlights

To generate a document preview with highlighted sections:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import HighlightOptions

def create_document_preview(file_path, num_sections=5, section_length=100):
    """
    Create a document preview by extracting highlights from various positions.
    """
    with Parser(file_path) as parser:
        # Get document text to determine positions
        text_reader = parser.get_text()
        
        if not text_reader:
            print("Text extraction not supported")
            return None
        
        full_text = text_reader
        text_length = len(full_text)
        
        # Calculate evenly distributed positions
        if text_length <= section_length:
            positions = [0]
        else:
            step = text_length // num_sections
            positions = [i * step for i in range(num_sections)]
        
        # Extract highlights
        preview_sections = []
        options = HighlightOptions(section_length)
        
        for pos in positions:
            highlight = parser.get_highlight(pos, True, options)
            if highlight:
                preview_sections.append(highlight.text)
        
        return {
            'document_length': text_length,
            'preview_sections': preview_sections,
            'full_preview': '
...
'.join(preview_sections)
        }

# Usage
preview = create_document_preview("sample.pdf", num_sections=3, section_length=150)

if preview:
    print(f"Document length: {preview['document_length']} characters
")
    print("Preview:")
    print(preview['full_preview'])
```

{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Creates a representative preview of the document by extracting highlights from evenly distributed positions.

## Extract highlights with word boundaries

To extract highlights respecting word boundaries:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import HighlightOptions

def extract_highlight_with_words(file_path, position, max_length=100):
    """
    Extract highlight ensuring complete words.
    """
    with Parser(file_path) as parser:
        # Extract more than needed
        options = HighlightOptions(max_length + 50)
        highlight = parser.get_highlight(position, True, options)
        
        if not highlight:
            return None
        
        text = highlight.text
        
        # Find last complete word within max_length
        if len(text) > max_length:
            truncated = text[:max_length]
            last_space = truncated.rfind(' ')
            if last_space > 0:
                text = truncated[:last_space] + "..."
        
        return {
            'position': highlight.position,
            'text': text,
            'complete_words': True
        }

# Usage
highlight = extract_highlight_with_words("sample.pdf", 500, 80)
if highlight:
    print(f"Position {highlight['position']}:")
    print(highlight['text'])
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-highlights/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts highlights that end at word boundaries, avoiding cut-off words.

## Highlight extraction for table of contents

To create a table of contents with text previews:
{{< tabs "example-7" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import HighlightOptions

def create_toc_with_previews(file_path, preview_length=60):
    """
    Create table of contents with text previews for each section.
    """
    with Parser(file_path) as parser:
        # Get TOC
        toc_items = parser.get_toc()
        
        if not toc_items:
            print("TOC extraction not supported")
            return []
        
        toc_with_previews = []
        options = HighlightOptions(preview_length)
        
        # Get text to find section positions
        text_reader = parser.get_text()
        if not text_reader:
            return []
        
        full_text = text_reader
        
        for item in toc_items:
            # Try to extract highlight for this TOC item
            # Use text search to find the section
            section_text = item.text
            position = full_text.find(section_text)
            
            if position >= 0:
                highlight = parser.get_highlight(position, True, options)
                preview = highlight.text if highlight else "No preview available"
            else:
                preview = "Section not found"
            
            toc_with_previews.append({
                'title': item.text,
                'depth': item.depth,
                'preview': preview
            })
        
        return toc_with_previews

# Usage
toc = create_toc_with_previews("SampleWithToc.pdf", preview_length=100)
for item in toc:
    indent = "  " * item['depth']
    print(f"{indent}{item['title']}")
    print(f"{indent}  Preview: {item['preview'][:50]}...")
```

{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Generates a table of contents with text previews for each section.

## Notes

- The `get_highlight()` method returns `None` if highlight extraction is not supported
- The second parameter (boolean) indicates whether to extract from fixed position (True) or dynamic position (False)
- Highlight length is specified in characters
- Use highlights with search results to show context
- Highlights are useful for creating document previews and snippets
- Consider word boundaries when displaying highlights to users

## Related pages

- [Search text in documents]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/search-text" >}})
- [Extract text areas]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-areas" >}})
- [Extract text in Accurate mode]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-in-accurate-mode" >}})

