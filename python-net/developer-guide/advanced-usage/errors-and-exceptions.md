---
id: errors-and-exceptions
url: parser/python-net/errors-and-exceptions
title: Errors and Exceptions
weight: 7
version: 25.12
description: "Handle parsing errors using groupdocs.parser.exceptions and add troubleshooting steps for GroupDocs.Parser for Python via .NET."
productName: GroupDocs.Parser for Python via .NET
hideChildren: false
toc: true
tags: python, parser, exceptions, troubleshooting, v25.12
---

GroupDocs.Parser raises exceptions from `groupdocs.parser.exceptions` for invalid inputs, unsupported formats, and access issues. Catch these exceptions to provide clear feedback and retry logic.

## Handle unsupported formats
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.exceptions import UnsupportedDocumentFormatException

try:
    with Parser("./unknown.xyz") as parser:
        reader = parser.get_text()
        print(reader if reader else "No text available.")
except Exception as e:
    print(e)
```

{{< /tab >}}
{{< /tabs >}}

## Troubleshooting checklist

- Confirm the file path exists and is readable by the current user.  
- If the document is password-protected, provide the password via `LoadOptions`.  
- For image-heavy PDFs on Linux, install `libgdiplus` and font packages.  
- Review the [API reference](https://reference.groupdocs.com/parser/python-net/) for method-specific exceptions and option requirements.  
- Check [system requirements]({{< ref "parser/python-net/system-requirements.md" >}}) and ensure the correct .NET runtime is installed.



