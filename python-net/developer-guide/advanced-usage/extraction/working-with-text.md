---
id: working-with-text
url: parser/python-net/working-with-text
title: Working with Text
weight: 2
version: 26.10
description: "Perform keyword or regex searches and inspect text extraction features with GroupDocs.Parser for Python via .NET."
productName: GroupDocs.Parser for Python via .NET
hideChildren: false
toc: true
tags: python, parser, text-search, formatted-text, v26.10
---

Beyond basic text extraction, you can search inside documents and control how text is returned. This page shows common tasks; consult the [API reference](https://reference.groupdocs.com/parser/python-net/) for the full list of text-related methods and options.

## Search for text
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import SearchOptions

with Parser("sample.pdf") as parser:
    # match_case, match_whole_word, use_regular_expression, search_by_pages
    options = SearchOptions(False, False, False, True)
    results = parser.search("harvest", options)
    if results is None:
        print("Search isn't supported for this format.")
    else:
        for result in results:
            print(f"Found on page {result.page_index + 1}: {result.text}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/working-with-text/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "search-text.txt" >}}  
```text
Found on page 1: Harvest
Found on page 2: Harvest
Found on page 2: Harvest
Found on page 2: harvest
Found on page 4: harvest
Found on page 4: Harvest
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/extraction/working-with-text/example-1/search-text.txt)
{{< /tab >}}
{{< /tabs >}}

### Notes

- `parser.search()` returns occurrences with matched text; set `search_by_pages` in `SearchOptions` to get page indexes.  
- For regex or case-sensitive searches, use search options from `groupdocs.parser.options` (see the API reference for available parameters).  
- Combine search with `get_text()` or `get_text()` per page to extract surrounding content.

## Extract formatted or structured text

GroupDocs.Parser can return formatted text (HTML/Markdown/plain) and structured text (headings, lists, tables) when supported by the format.
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import FormattedTextOptions, FormattedTextMode

with Parser("sample.pptx") as parser:
    # Extract formatted text as Markdown (HTML and PLAIN_TEXT modes are also available)
    reader = parser.get_formatted_text(FormattedTextOptions(FormattedTextMode.MARKDOWN))
    print(reader.read_to_end() if reader is not None else "Formatted text isn't available.")
```

{{< /tab >}}
{{< tab "sample.pptx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pptx](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/working-with-text/sample.pptx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "extract-formatted-or-structured-text.txt" >}}  
```text
Autumn Harvest 2025

Seasonal lines from Harbor Lane Provisions for grocers, cafes and restaurants.

What is in the range

Autumn Harvest spiced roast from Northwind Coffee Roasters
Apple butter, cranberry relish and fig preserve
Squash soup, muesli and a fruit loaf from local producers

[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/extraction/working-with-text/example-2/extract-formatted-or-structured-text.txt)
{{< /tab >}}
{{< /tabs >}}

See the [API reference](https://reference.groupdocs.com/parser/python-net/) for the formatted and structured text methods supported in Python via .NET.

