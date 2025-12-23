---
id: working-with-containers
url: parser/python-net/working-with-containers
title: Working with Containers
weight: 4
version: 25.12
description: "Handle nested attachments, archives, and email stores with GroupDocs.Parser for Python via .NET."
productName: GroupDocs.Parser for Python via .NET
hideChildren: false
toc: true
tags: python, parser, containers, attachments, v25.12
---

Container-aware parsing lets you open archive-like formats (ZIP, RAR, 7z, TAR), Outlook stores (PST/OST), PDF portfolios, and email attachments, then process their contents using the same parser API.

## Iterate nested items safely
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.exceptions import UnsupportedDocumentFormatException

def print_text(item_path, parser):
    reader = parser.get_text()
    print(f"{item_path}: {reader if reader else '[no text]'}")

with Parser("portfolio.pdf") as parser:
    container = parser.get_container()
    if container is None:
        print("Container extraction isn't supported for this format.")
    else:
        for item in container:
            try:
                with item.open_parser() as inner_parser:
                    print_text(item.file_path, inner_parser)
            except UnsupportedDocumentFormatException:
                print(f"{item.file_path}: format is not supported.")
```

{{< /tab >}}
{{< tab "portfolio.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [portfolio.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/working-with-containers/portfolio.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

### Tips

- Containers can be nested; call `open_parser()` recursively when needed.  
- Use `item.file_type` and `item.metadata` (when available) to decide how to process each attachment.  
- If `get_container()` returns `None`, the format does not expose attachments; continue with other extraction methods.

