---
id: errors-and-exceptions
url: parser/python-net/errors-and-exceptions
title: Errors and Exceptions
weight: 7
version: 26.10
description: "Handle parsing errors using groupdocs.parser.exceptions and add troubleshooting steps for GroupDocs.Parser for Python via .NET."
productName: GroupDocs.Parser for Python via .NET
hideChildren: false
toc: true
tags: python, parser, exceptions, troubleshooting, v26.10
---

GroupDocs.Parser raises exceptions from `groupdocs.parser.exceptions` for invalid inputs, unsupported formats, and access issues. Catch these exceptions to provide clear feedback and retry logic.

## Exception classes

Every error the engine reports is a `GroupDocsParserException` or one of its subclasses:

| Exception | Raised when |
|---|---|
| `InvalidPasswordException` | a protected document is opened without the right password |
| `CorruptedDocumentException` | the document is damaged, for example a truncated file |
| `UnsupportedDocumentFormatException` | the file's content is not a format GroupDocs.Parser reads |
| `GroupDocsParserException` | any other engine error; it is also the base class of the three above |

Catch `GroupDocsParserException` to handle every engine error in one place. Version 25.12 raised `RuntimeError` for all of them instead. These classes do not derive from `RuntimeError`, so code written for 25.12 that catches `RuntimeError` must catch `GroupDocsParserException` instead.

## Handle unsupported formats
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.exceptions import UnsupportedDocumentFormatException

try:
    with Parser("./unknown.xyz") as parser:
        reader = parser.get_text()
        print(reader.read_to_end() if reader is not None else "No text available.")
except UnsupportedDocumentFormatException:
    print("The document format isn't supported.")
except FileNotFoundError:
    print("The file wasn't found.")
```

{{< /tab >}}
{{< /tabs >}}

## Troubleshooting checklist

- Confirm the file path exists and is readable by the current user.  
- If the document is password-protected, provide the password via `LoadOptions`.  
- On Linux, install ICU, fontconfig, the Liberation fonts and libgdiplus (`apt-get install libicu-dev libfontconfig1 fonts-liberation libgdiplus`). Without ICU the process aborts on the first call with "Couldn't find a valid ICU package" instead of raising an exception.  
- `The type initializer for 'Gdip' threw an exception` or `DllNotFoundException: libgdiplus` on Linux or macOS: install libgdiplus (`apt-get install libgdiplus`, or `brew install mono-libgdiplus` on macOS). Presentations need it: page previews, ODP and OTP files, and PowerPoint files with tables or hyperlinks.  
- Review the [API reference](https://reference.groupdocs.com/parser/python-net/) for method-specific exceptions and option requirements.  
- Check [system requirements]({{< ref "parser/python-net/getting-started/system-requirements.md" >}}): the wheel bundles its own .NET runtime, so no separate .NET install is needed, but it requires 64-bit Python, glibc 2.27+ on Linux and macOS 13+.  
- On macOS 13.0-13.2, OCR raises `GroupDocsParserException` naming `Microsoft.ML.OnnxRuntime.NativeMethods` and `DllNotFoundException`: OCR needs macOS 13.3 or newer. Every other feature works there.
- On macOS, a process with no logged-in desktop session, such as a CI runner or a background service, may print `XType: com.apple.fonts is not accessible.` and `XType: XTFontStaticRegistry is enabled.` (or `XType: Using static font registry.`) to stderr when it first loads fonts. These are macOS messages, not errors: macOS then reads the system font folders directly, and parsing is unaffected.



