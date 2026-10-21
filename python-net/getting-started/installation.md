---
id: installation
url: parser/python-net/installation
title: Install GroupDocs.Parser for Python via .NET
linkTitle: Installation
weight: 4
description: "Install GroupDocs.Parser for Python via .NET on Windows, Linux, or macOS - from PyPI or from a pre-downloaded wheel, including Intel and Apple Silicon builds."
keywords: install, installation, pip, pypi, wheel, whl, Windows, Linux, macOS, Apple Silicon, manylinux, requirements.txt, GroupDocs.Parser, python
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
structuredData:
  showOrganization: True
  application:
    name: Document Parser Tool
    description: The product allows to extract text, images, metadata, tables, and structured data from PDF, Word, Excel, PowerPoint and 65+ other file formats. Parser API also supports template-based extraction and container processing.
    productCode: parser
    productPlatform: python-net
  howTo:
    name: How to install Parser API
    description: Learn how to install GroupDocs.Parser into Python
    steps:
      - name: Install from PyPI
        text: Install the package using pip from PyPI repository
      - name: Download from Official Website
        text: Download the OS-specific package from GroupDocs releases website
---

GroupDocs.Parser for Python via .NET is distributed as a pre-built wheel on [PyPI](https://pypi.org/project/groupdocs-parser-net/). The PyPI index hosts a separate wheel for each supported platform, and `pip` picks the correct one automatically. Each wheel is self-contained: it bundles the embedded .NET runtime and every managed dependency, so no Microsoft Office, OpenOffice, or separate runtime install is required.

{{< alert style="info" >}}
Before you install, review the [System Requirements]({{< ref "parser/python-net/system-requirements" >}}). The wheels need 64-bit Python, Linux with glibc 2.27 or newer, or macOS 13 or newer, and pip 20.3 or newer to install. On macOS, OCR (text from scanned pages and images) needs macOS 13.3 or newer. On Linux, install ICU, fontconfig, the Liberation fonts and libgdiplus, and on macOS mono-libgdiplus (see [Platform Prerequisites](#platform-prerequisites)).
{{< /alert >}}

## Install Package from PyPI

Open a terminal and run the install command for your platform:

{{< tabs "install-pypi">}}
{{< tab "Windows" >}}
```ps
py -m pip install groupdocs-parser-net
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 -m pip install groupdocs-parser-net
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 -m pip install groupdocs-parser-net
```
{{< /tab >}}
{{< /tabs >}}

After running the command you should see output similar to:

```bash
Collecting groupdocs-parser-net
  Downloading groupdocs_parser_net-26.10.0-py3-none-win_amd64.whl (301.4 MB)
Installing collected packages: groupdocs-parser-net
Successfully installed groupdocs-parser-net-26.10.0
```

The wheel file name includes a platform suffix that matches your operating system - for example `manylinux_2_27_x86_64` on Linux, `macosx_13_0_arm64` on Apple Silicon, or `win_amd64` on 64-bit Windows.

## Add the Package to `requirements.txt`

For reproducible environments, pin the package version in your `requirements.txt`:

```txt
groupdocs-parser-net==26.10.0
```

Then install all dependencies in one step:

```bash
pip install -r requirements.txt
```

## Install from a Pre-Downloaded Wheel

If your build environment cannot reach PyPI, download the appropriate wheel from the [GroupDocs Releases website](https://releases.groupdocs.com/parser/python-net/) and install it locally. Wheels are published for the following four platforms:

| Platform | Wheel file name ends with | Oldest system |
| --- | --- | --- |
| Windows 64-bit | `py3-none-win_amd64.whl` | Windows 10, Windows Server 2016 |
| Linux x64 (glibc) | `py3-none-manylinux_2_27_x86_64.whl` | glibc 2.27 (Ubuntu 18.04, Debian 10, RHEL 8) |
| macOS Apple Silicon (M-series) | `py3-none-macosx_13_0_arm64.whl` | macOS 13 (13.3 for OCR) |
| macOS Intel | `py3-none-macosx_13_0_x86_64.whl` | macOS 13 (13.3 for OCR) |

Since 26.10 the file names state the oldest system each wheel runs on, so pip 20.3 or newer refuses an older one up front instead of installing a runtime that cannot start. Version 25.12 was tagged `manylinux1_x86_64`, `macosx_10_14_x86_64` and `macosx_11_0_arm64`, and also shipped a 32-bit Windows wheel; 26.10 has no 32-bit wheel.

Place the downloaded wheel into your project folder, then install it:

{{< tabs "install-wheel">}}
{{< tab "Windows (64-bit)" >}}
```ps
py -m pip install .\groupdocs_parser_net-26.10.0-py3-none-win_amd64.whl
```
{{< /tab >}}
{{< tab "Linux (glibc)" >}}
```bash
python3 -m pip install ./groupdocs_parser_net-26.10.0-py3-none-manylinux_2_27_x86_64.whl
```
{{< /tab >}}
{{< tab "macOS (Intel)" >}}
```bash
python3 -m pip install ./groupdocs_parser_net-26.10.0-py3-none-macosx_13_0_x86_64.whl
```
{{< /tab >}}
{{< tab "macOS (Apple Silicon)" >}}
```bash
python3 -m pip install ./groupdocs_parser_net-26.10.0-py3-none-macosx_13_0_arm64.whl
```
{{< /tab >}}
{{< /tabs >}}

Name the wheel file explicitly: PowerShell does not expand a `*` wildcard for a native command, so `pip install groupdocs_parser_net-*.whl` fails there.

Expected output (Linux):

```bash
Processing ./groupdocs_parser_net-26.10.0-py3-none-manylinux_2_27_x86_64.whl
Installing collected packages: groupdocs-parser-net
Successfully installed groupdocs-parser-net-26.10.0
```

## Platform Prerequisites

On Windows no extra steps are required. On Linux, install ICU, fontconfig, the Liberation fonts and libgdiplus:

```bash
sudo apt-get update
sudo apt-get install -y libicu-dev libfontconfig1 fonts-liberation libgdiplus
```

On macOS, install libgdiplus:

```bash
brew install mono-libgdiplus
```

libgdiplus serves presentations: page previews, ODP and OTP files, and PowerPoint files with tables or hyperlinks. The Microsoft core fonts are not needed. The [System Requirements]({{< ref "parser/python-net/system-requirements" >}}) page explains what each package is for.

## Verify the Installation

Confirm the package imports correctly:

{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
# The import fails if the package is not installed correctly
from groupdocs.parser import Parser

print("GroupDocs.Parser for Python via .NET installed successfully!")
```

{{< /tab >}}
{{< /tabs >}}

The package also installs the `groupdocs-parser` command (see [Command Line Interface]({{< ref "parser/python-net/developer-guide/command-line-interface" >}})), which prints the installed version:

```bash
groupdocs-parser --version
```

```bash
groupdocs-parser 26.10.0
```

You can also run `pip show groupdocs-parser-net` to confirm the version and location.

## Quick Start Example

Here's a simple example to extract text from a document:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Extract text from the document
    text = parser.get_text()

    if text is None:
        print("Text extraction isn't supported for this format")
    else:
        # Print the extracted text
        print(text)
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/getting-started/installation/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

The first `Parser(...)` in a process takes 20-30 seconds while the engine loads; later documents open in a fraction of a second.

## Next Steps

- Follow the [Quick Start Guide]({{< ref "parser/python-net/getting-started/quick-start" >}}) to extract text, images and metadata.
- Read the [Features Overview]({{< ref "parser/python-net/getting-started/features-overview" >}}) to see everything you can do.
- Clone the [examples repository](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET) and read [How to Run Examples]({{< ref "parser/python-net/getting-started/how-to-run-examples" >}}) to try every documented scenario locally.
