---
id: search-text
url: parser/python-net/search-text
title: Search text
weight: 4
description: "Learn how to search for keywords and use regular expressions to find text in documents using GroupDocs.Parser for Python via .NET. Search text with case sensitivity and whole word options."
keywords: search text, search text from documents, text search, keyword search, regex search, document search
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser provides powerful text search functionality with support for keywords, regular expressions, case-sensitive search, and highlighting.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample documents for testing
- Basic understanding of regular expressions (for advanced searches)

## Search text by keyword

To search for a specific keyword in a document:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Search for a keyword
    search_results = parser.search("invoice")
    
    # Check if search is supported
    if search_results is None:
        print("Search isn't supported")
    else:
        # Iterate over search results
        for result in search_results:
            # Print position and found text
            print(f"At {result.position}: {result.text}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/search-text/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** The method returns a collection of `SearchResult` objects, each containing the position and text of every occurrence of the keyword.

## Search with regular expressions

To search using regular expressions:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import SearchOptions

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Create search options for regex search
    # Parameters: match_case, match_whole_word, use_regular_expression
    options = SearchOptions(True, False, True)
    
    # Search with a regular expression (case-sensitive)
    search_results = parser.search(r"page number: \d+", options)
    
    # Check if search is supported
    if search_results is None:
        print("Search isn't supported")
    else:
        # Iterate over search results
        for result in search_results:
            print(f"At {result.position}: {result.text}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/search-text/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Finds all text matching the regular expression pattern with the specified options.

## Search with case sensitivity and whole word matching
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import SearchOptions

# Create an instance of Parser class
with Parser("./sample.docx") as parser:
    # Search for exact word match (case-insensitive, whole word)
    # Parameters: match_case=False, match_whole_word=True, use_regular_expression=False
    options = SearchOptions(False, True, False)
    
    search_results = parser.search("invoice", options)
    
    if search_results:
        print(f"Found {len(list(search_results))} occurrences of 'invoice' as whole word")
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/search-text/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Search text with highlights

To search and extract surrounding text (highlights):
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import SearchOptions, HighlightOptions

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Create highlight options (extract 15 characters around the match)
    highlight_options = HighlightOptions(15)
    
    # Create search options with highlights
    search_options = SearchOptions(
        match_case=False,
        match_whole_word=False,
        use_regular_expression=False,
        left_highlight_options=highlight_options,
        right_highlight_options=highlight_options
    )
    
    # Search with highlights
    search_results = parser.search("lorem", search_options)
    
    if search_results is None:
        print("Search isn't supported")
    else:
        # Iterate over search results and print with highlights
        for result in search_results:
            left_text = result.left_highlight_item.text if result.left_highlight_item else ""
            right_text = result.right_highlight_item.text if result.right_highlight_item else ""
            print(f"{left_text}[{result.text}]{right_text}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/search-text/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns search results with context from surrounding text on both sides of the match.

## Search text with page numbers

To search and get page numbers where text appears:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import SearchOptions

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Create search options with page search enabled
    # Parameters: match_case, match_whole_word, use_regular_expression, search_by_pages
    options = SearchOptions(False, False, False, True)
    
    # Search with page numbers
    search_results = parser.search("lorem", options)
    
    if search_results is None:
        print("Search isn't supported")
    else:
        # Iterate over search results
        for result in search_results:
            # Print position, page number, and found text
            print(f"At {result.position} (page {result.page_index + 1}): {result.text}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/search-text/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Each search result includes the page index where the text was found.

## Advanced search example

Combine multiple search techniques:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import SearchOptions, HighlightOptions

def advanced_search(file_path, pattern, case_sensitive=False, use_regex=False):
    """
    Perform advanced text search with highlights and page numbers.
    """
    try:
        with Parser(file_path) as parser:
            # Configure highlight options
            highlight_opts = HighlightOptions(20)
            
            # Configure search options
            search_opts = SearchOptions(
                match_case=case_sensitive,
                match_whole_word=False,
                use_regular_expression=use_regex,
                search_by_pages=True,
                left_highlight_options=highlight_opts,
                right_highlight_options=highlight_opts
            )
            
            # Perform search
            results = parser.search(pattern, search_opts)
            
            if results is None:
                print("Search not supported for this document")
                return []
            
            # Process results
            found_items = []
            for result in results:
                found_items.append({
                    'text': result.text,
                    'position': result.position,
                    'page': result.page_index + 1,
                    'left_context': result.left_highlight_item.text if result.left_highlight_item else "",
                    'right_context': result.right_highlight_item.text if result.right_highlight_item else ""
                })
            
            return found_items
            
    except Exception as e:
        print(f"Error during search: {e}")
        return []

# Usage
results = advanced_search("sample.pdf", r"\d{4}-\d{2}-\d{2}", use_regex=True)
for item in results:
    print(f"Page {item['page']}: {item['left_context']}[{item['text']}]{item['right_context']}")
```

{{< /tab >}}
{{< /tabs >}}

## Notes

- The `search()` method returns `None` if search is not supported for the document format
- Use `parser.features.search` to check if search is available before calling `search()`
- Regular expressions follow Python's regex syntax
- Highlight extraction adds minimal performance overhead
- Page-based search (`search_by_pages=True`) is useful for large documents

## Related pages

- [Extract text in Accurate mode]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-in-accurate-mode" >}})
- [Extract text areas]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-areas" >}})
- [Extract highlights]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-highlights" >}})

