---
id: installation
url: parser/python-net/installation
title: Install GroupDocs.Parser for Python via .NET 
weight: 4
description: ""
keywords:
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
structuredData:
  showOrganization: True
  application:
    name: Document Parser Tool
    description: The product allows to extract text, images, metadata, tables, and structured data from PDF, Word, Excel, PowerPoint and 50+ other file formats. Parser API also supports template-based extraction and container processing.
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

### Install using PyPI

All Python packages are hosted at [PyPI](https://pypi.org/project/groupdocs-parser-net/). You can easily reference GroupDocs.Parser for Python via .NET API directly in your Python project by installing it with the following command.

```shell
pip install groupdocs-parser-net
```

## Download Package from Official Website

To download the GroupDocs.Parser package for your operating system, please visit the official [GroupDocs Releases website](https://releases.groupdocs.com/parser/python-net/). Currently, OS-specific packages are available for different platforms:

- **Windows 64-bit:** Package name ends with `amd64.whl`
- **Linux 64-bit:** Package name ends with `x86_64.whl`
- **macOS Intel Silicon:** Package name ends with `macosx_10_14_x86_64.whl`

Choose the appropriate package based on your system's architecture.

### Copy Downloaded File

Copy the downloaded file into your project folder.

### Install Package Using Pip

To install package run the command specific to your operation system. 

Windows 64-bit

```bash
py -m pip install groupdocs_parser_net-24.12-py3-none-win_amd64.whl
```

Linux 64-bit

```bash
python3 -m pip install groupdocs_parser_net-24.12-py3-none-manylinux1_x86_64.whl
```

macOS Intel Silicon

```bash
python3 -m pip install groupdocs_parser_net-24.12-py3-none-macosx_10_14_x86_64.whl
```

Expected output:

```bash
Processing groupdocs_parser_net-24.12-py3-none-*.whl
Installing collected packages: groupdocs-parser-net
Successfully installed groupdocs-parser-net-24.12
```

## Verify Installation

After installation, you can verify that GroupDocs.Parser is correctly installed by running the following Python code:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def verify_installation():
    # Check if the module is loaded correctly
    print("GroupDocs.Parser for Python via .NET installed successfully!")

if __name__ == "__main__":
    verify_installation()
```

{{< /tab >}}
{{< /tabs >}}

## Quick Start Example

Here's a simple example to extract text from a document:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def extract_text_quick_start():
    # Create an instance of Parser class
    with Parser("./sample.pdf") as parser:
        # Extract text from the document
        text_reader = parser.get_text()
        
        if text_reader is not None:
            # Print the extracted text
            print(text_reader)
        else:
            print("Text extraction isn't supported for this format")

if __name__ == "__main__":
    extract_text_quick_start()
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/getting-started/installation/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## System Requirements

Before installing GroupDocs.Parser for Python via .NET, ensure your system meets the following requirements:

- **Python:** Version 3.5 or higher
- **Operating Systems:** Windows, Linux, macOS
- **Dependencies:** All required dependencies are automatically installed via pip

For more details, see the [System Requirements]({{< ref "parser/python-net/system-requirements" >}}) page.
