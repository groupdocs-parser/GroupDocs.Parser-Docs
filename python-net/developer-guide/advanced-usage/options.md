---
id: options
url: parser/python-net/options
title: Options and Configuration
weight: 6
version: 25.12
description: "Control parsing behavior with options from groupdocs.parser.options in GroupDocs.Parser for Python via .NET."
productName: GroupDocs.Parser for Python via .NET
hideChildren: false
toc: true
tags: python, parser, options, configuration, v25.12
---

Most parsing scenarios work with defaults, but `groupdocs.parser.options` exposes classes to control how documents are opened and processed. Use these options to handle passwords, guide format detection, or tune search and text extraction modes.

## Apply load options
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import LoadOptions

# Pass a password and open from a stream
with open("input/protected.pdf", "rb") as stream:
    options = LoadOptions("secret-password")
    with Parser(stream, options) as parser:
        reader = parser.get_text()
        print(reader if reader else "Text extraction isn't supported.")
```

{{< /tab >}}
{{< tab "protected.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [protected.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/options/input/protected.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## When to use other options

- **Search options** — control case-sensitivity or regex mode when calling search APIs.  
- **Text/formatting options** — request formatted text (HTML/Markdown/plain) where supported.  
- **Parser settings** — adjust resource loading or timeouts for specific environments.

Check the [`groupdocs.parser.options` namespace](https://reference.groupdocs.com/parser/python-net/groupdocs.parser.options/) for the complete list of option classes and their parameters, and pass them to the corresponding methods as documented in the API reference.



