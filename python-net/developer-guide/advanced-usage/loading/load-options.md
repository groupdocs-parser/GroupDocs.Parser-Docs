---
id: load-options
url: parser/python-net/load-options
title: Load Options
weight: 1
version: 25.12
description: "Open password-protected files and streams using load options in GroupDocs.Parser for Python via .NET."
productName: GroupDocs.Parser for Python via .NET
hideChildren: false
toc: true
---

Use `groupdocs.parser.options.LoadOptions` to pass extra parameters when opening a document, such as a password or file type when loading from a stream.

## Open a password-protected document
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import LoadOptions

options = LoadOptions("secret-password")

with Parser("protected.pdf", options) as parser:
    reader = parser.get_text()
    print(reader if reader else "Text extraction isn't supported.")
```

{{< /tab >}}
{{< tab "protected.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [protected.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-options/protected.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Load from a stream

When you read from a stream, provide `LoadOptions` so the parser knows how to open the content (for example, password-protected PDF streams).
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import LoadOptions

with open("resources/protected.pdf", "rb") as stream:
    options = LoadOptions("secret-password")
    with Parser(stream, options) as parser:
        reader = parser.get_text()
        print(reader if reader else "Text extraction isn't supported.")
```

{{< /tab >}}
{{< tab "protected.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [protected.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-options/protected.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

### Tips

- Always dispose of streams and parsers with `with` statements.  
- If a password is wrong, the parser raises an exception (see [errors and exceptions]({{< ref "parser/python-net/developer-guide/advanced-usage/loading/load-password-protected-documents.md" >}})).  
- For additional options (e.g., file type hints), see the [`LoadOptions` API reference](https://reference.groupdocs.com/parser/python-net/groupdocs.parser.options/loadoptions/).

