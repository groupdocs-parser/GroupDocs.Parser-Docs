---
id: working-with-text
url: parser/python-net/working-with-text
title: Working with Text
weight: 2
version: 25.12
description: "Perform keyword or regex searches and inspect text extraction features with GroupDocs.Parser for Python via .NET."
productName: GroupDocs.Parser for Python via .NET
hideChildren: false
toc: true
tags: python, parser, text-search, formatted-text, v25.12
---

Beyond basic text extraction, you can search inside documents and control how text is returned. This page shows common tasks; consult the [API reference](https://reference.groupdocs.com/parser/python-net/) for the full list of text-related methods and options.

## Search for text
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("sample.pdf") as parser:
    results = parser.search("GroupDocs")
    if results is None:
        print("Search isn't supported for this format.")
    else:
        for result in results:
            print(f"Found on page {result.page.index + 1}: {result.text}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/working-with-text/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

### Notes

- `parser.search()` returns occurrences with page indexes and matched text.  
- For regex or case-sensitive searches, use search options from `groupdocs.parser.options` (see the API reference for available parameters).  
- Combine search with `get_text()` or `get_text()` per page to extract surrounding content.

## Extract formatted or structured text

GroupDocs.Parser can return formatted text (HTML/Markdown/plain) and structured text (headings, lists, tables) when supported by the format.
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("sample.pptx") as parser:
    # Use format-specific text extraction methods exposed in the API reference
    reader = parser.get_text()
    print(reader if reader else "Formatted text isn't available.")
```

{{< /tab >}}
{{< tab "sample.pptx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pptx](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/working-with-text/sample.pptx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

See the [API reference](https://reference.groupdocs.com/parser/python-net/) for the formatted and structured text methods supported in Python via .NET.

