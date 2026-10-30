---
id: hello-world
url: parser/python-net/hello-world
title: Hello, World!
second_title: Basic Example of Using GroupDocs.Parser for Python via .NET
linkTitle: Hello World
weight: 2
keywords: "hello world, example, get started, extract text"
description: Quickly get started with GroupDocs.Parser for Python via .NET by creating and running a simple example.
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
---

## Introduction
A "Hello, World!" example is often the first step when exploring GroupDocs.Parser for Python via .NET. It serves as a simple test to confirm that your development environment is correctly set up and that the library is functioning as expected.

## Overview
GroupDocs.Parser for Python via .NET extracts text, tables, images, metadata and more from documents of various formats. A wide range of [supported formats](/parser/python-net/supported-document-formats/) makes it versatile for different use cases.

## How to extract text
The following steps demonstrate how to extract the text of a document using GroupDocs.Parser for Python via .NET:

1. Import the `Parser` class from `groupdocs.parser`.
2. Open a `Parser` with the path to the sample document.
3. Call `get_text()` to extract the text.
4. Print the result.

## Complete example
The example below extracts the text of a four-page PDF report and prints it:

{{< tabs "code-example-hello-world" >}}
{{< tab "hello_world.py" >}}
```python
from groupdocs.parser import Parser

def hello_world():
    # Open the document and extract its text
    with Parser("./sample.pdf") as parser:
        text = parser.get_text()

        # get_text() returns None for a format without text extraction
        if text is None:
            print("Text extraction isn't supported for this format")
        else:
            print(text)

if __name__ == "__main__":
    hello_world()
```
{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
`sample.pdf` is the sample file used in this example. Click [here](/parser/python-net/_sample_files/getting-started/hello-world/sample.pdf) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "hello-world.txt" >}}  
```text
Investor relations


Annual Report 2025  |  Port Ellery
Annual Report 2025


1. Overview
Harbor Lane Provisions supplies specialty food to 1,265 independent grocers, cafes and restaurants from its
Quayside warehouse in Port Ellery. In 2025 net revenue grew 15.1% to $21.4 million, driven by the Autumn
[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/getting-started/hello-world/hello_world/hello-world.txt)
{{< /tab >}}
{{< /tabs >}}

`get_text()` returns the text as a `str`. To extract one page, pass its index: `get_text(0)` returns the text of the first page. The first `Parser` in a process takes 20-30 seconds while the engine loads; documents opened after that take a fraction of a second.

## Additional resources
This demo references the GroupDocs.Parser for Python via .NET [code samples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET/).
