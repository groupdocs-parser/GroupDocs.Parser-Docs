---
id: extract-data-from-attachments-and-zip-archives
url: parser/python-net/extract-data-from-attachments-and-zip-archives
title: Extract Data from Attachments and ZIP Archives
weight: 9
version: 25.12
description: "Work with containers such as ZIP archives, email stores, and PDF portfolios using GroupDocs.Parser for Python via .NET."
productName: GroupDocs.Parser for Python via .NET
hideChildren: false
toc: true
tags: python, parser, attachments, zip, container, v25.12
---

Use container extraction to open archive-like formats (ZIP, RAR, TAR), Outlook stores (PST/OST), PDF portfolios, and email attachments, then parse their contents.

## Extract and parse attachments
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.exceptions import UnsupportedDocumentFormatException

with Parser("./archive.zip") as parser:
    attachments = parser.get_container()
    if attachments is None:
        print("Container extraction isn't supported for this format.")
    else:
        for item in attachments:
            print(item.file_path)
            try:
                with item.open_parser() as attachment_parser:
                    reader = attachment_parser.get_text()
                    print(reader if reader else "No text available.")
            except Exception as e:
                print(e)
```

{{< /tab >}}
{{< tab "archive.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [archive.zip](/parser/python-net/_sample_files/developer-guide/basic-usage/extract-data-from-attachments-and-zip-archives/archive.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

### Steps

1. Instantiate `Parser` for the container file.  
2. Call `get_container()` to list `ContainerItem` entries.  
3. For each item, use `open_parser()` to create a `Parser` over the embedded content and reuse standard extraction methods (`get_text`, `get_images`, `get_metadata`, etc.).  
4. Handle unsupported formats with `UnsupportedDocumentFormatException`.

For working with attachments in complex scenarios (nested containers, load options), see the [advanced containers guide]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-archives/_index.md" >}}).
---
id: extract-data-from-attachments-and-zip-archives
url: parser/python-net/extract-data-from-attachments-and-zip-archives
title: Extract Data from Attachments and ZIP Archives
weight: 9
version: 25.12
description: "Work with containers such as ZIP archives, email stores, and PDF portfolios using GroupDocs.Parser for Python via .NET."
productName: GroupDocs.Parser for Python via .NET
hideChildren: false
toc: true
tags: python, parser, attachments, zip, container, v25.12
---

Use container extraction to open archive-like formats (ZIP, RAR, TAR), Outlook stores (PST/OST), PDF portfolios, and email attachments, then parse their contents.

## Extract and parse attachments
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.exceptions import UnsupportedDocumentFormatException

with Parser("./archive.zip") as parser:
    attachments = parser.get_container()
    if attachments is None:
        print("Container extraction isn't supported for this format.")
    else:
        for item in attachments:
            print(item.file_path)
            try:
                with item.open_parser() as attachment_parser:
                    reader = attachment_parser.get_text()
                    print(reader if reader else "No text available.")
            except UnsupportedDocumentFormatException:
                print("Item format is not supported.")
```

{{< /tab >}}
{{< tab "archive.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [archive.zip](/parser/python-net/_sample_files/developer-guide/basic-usage/extract-data-from-attachments-and-zip-archives/archive.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

### Steps

1. Instantiate `Parser` for the container file.  
2. Call `get_container()` to list `ContainerItem` entries.  
3. For each item, use `open_parser()` to create a `Parser` over the embedded content and reuse standard extraction methods (`get_text`, `get_images`, `get_metadata`, etc.).  
4. Handle unsupported formats with `UnsupportedDocumentFormatException`.

For working with attachments in complex scenarios (nested containers, load options), follow the `.NET` [advanced container guide]({{< ref "parser/net/developer-guide/advanced-usage/working-with-zip-archives-and-attachments/_index.md" >}}); the same workflow applies in Python.



