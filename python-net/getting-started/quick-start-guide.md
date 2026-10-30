---
id: quick-start-guide
url: parser/python-net/quick-start-guide
title: Quick Start Guide
linkTitle: Quick Start Guide
second_title: A simple example of how to use GroupDocs.Parser for Python via .NET
weight: 4
keywords: quick start, hello world, get started, extract text, extract metadata, extract tables, pip install, venv, virtual environment, GroupDocs.Parser, python
description: "Set up a virtual environment, install groupdocs-parser-net, and run three minimal examples — extract text, read metadata, and extract tables — in under five minutes."
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
aliases:
    - /parser/python-net/quick-start/
---

This guide provides a quick overview of how to set up and start using GroupDocs.Parser for Python via .NET. The library opens a document and extracts its text, tables, images, metadata and more, whatever the format.

## Prerequisites

To proceed, make sure you have:

1. **Configured** environment as described in the [System Requirements]({{< ref "parser/python-net/getting-started/system-requirements.md" >}}) topic.
2. **Optionally** you may [Get a Temporary License](https://purchase.groupdocs.com/temporary-license/) to test all the product features.

## Set Up Your Development Environment

For best practices, use a virtual environment to manage dependencies in Python applications. Learn more about virtual environments at the [Create and Use Virtual Environments](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/#create-and-use-virtual-environments) documentation topic.

### Create and Activate a Virtual Environment

Create a virtual environment:

{{< tabs "example1">}}
{{< tab "Windows" >}}
```ps
py -m venv .venv
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 -m venv .venv
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 -m venv .venv
```
{{< /tab >}}
{{< /tabs >}}

Activate a virtual environment:

{{< tabs "example2">}}
{{< tab "Windows" >}}
```ps
.venv\Scripts\activate
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
source .venv/bin/activate
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
source .venv/bin/activate
```
{{< /tab >}}
{{< /tabs >}}

### Install `groupdocs-parser-net` Package

After activating the virtual environment, run the following command in your terminal to install the latest version of the package:

{{< tabs "example3">}}
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

Ensure the package is installed successfully. You should see the message

```bash
Successfully installed groupdocs-parser-net-*
```

{{< alert style="info" >}}
Version 26.10 is not on PyPI yet: until it is, `pip install groupdocs-parser-net` installs 25.12. Install the 26.10 wheel for your platform from the [GroupDocs Releases website](https://releases.groupdocs.com/parser/python-net/) instead, as described in [Install from a Pre-Downloaded Wheel]({{< ref "parser/python-net/getting-started/installation.md#install-from-a-pre-downloaded-wheel" >}}).
{{< /alert >}}

## Example 1: Extract text

To quickly test the library, let's extract the text of a PDF report.

{{< tabs "demo_app_extract_text">}}
{{< tab "extract_text.py" >}}
```python
import os
from groupdocs.parser import Parser, License

def extract_text():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Parser.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Open the document and extract its text
    with Parser("./sample.pdf") as parser:
        text = parser.get_text()

        if text is None:
            print("Text extraction isn't supported for this format")
        else:
            print(text)

if __name__ == "__main__":
    extract_text()
```
{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
`sample.pdf` is the sample file used in this example. Click [here](/parser/python-net/_sample_files/getting-started/quick-start-guide/sample.pdf) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "extract-text.txt" >}}  
```text
Investor relations


Annual Report 2025  |  Port Ellery
Annual Report 2025


1. Overview
Harbor Lane Provisions supplies specialty food to 1,265 independent grocers, cafes and restaurants from its
Quayside warehouse in Port Ellery. In 2025 net revenue grew 15.1% to $21.4 million, driven by the Autumn
[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/getting-started/quick-start-guide/extract_text/extract-text.txt)
{{< /tab >}}
{{< /tabs >}}

Your folder tree should look similar to the following directory structure:

```Directory
📂 demo-app
 ├──extract_text.py
 ├──sample.pdf
 └──GroupDocs.Parser.lic (Optionally)
```

### Run the App

{{< tabs "run_the_app_extract_text">}}
{{< tab "Windows" >}}
```ps
py extract_text.py
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 extract_text.py
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 extract_text.py
```
{{< /tab >}}
{{< /tabs >}}

After running the app you can deactivate the virtual environment by executing `deactivate` or closing your shell.

### Explanation
- `Parser("./sample.pdf")`: Opens the document. Used as a context manager so the native document handle is released.
- `parser.get_text()`: Extracts the text of the whole document as a `str`, or returns `None` for a format that has no text extraction. Pass a page index, as in `get_text(0)`, to extract one page.

## Example 2: Read metadata

In this example we read the document properties of the same report: its title, subject, author and dates.

{{< tabs "demo_app_extract_metadata">}}
{{< tab "extract_metadata.py" >}}
```python
import os
from groupdocs.parser import Parser, License

def extract_metadata():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Parser.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Open the document and print each metadata item
    with Parser("./sample.pdf") as parser:
        metadata = parser.get_metadata()

        if metadata is None:
            print("Metadata extraction isn't supported for this format")
        else:
            for item in metadata:
                print(item.name, "=", item.value)

if __name__ == "__main__":
    extract_metadata()
```
{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
`sample.pdf` is the sample file used in this example. Click [here](/parser/python-net/_sample_files/getting-started/quick-start-guide/sample.pdf) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "extract-metadata.txt" >}}  
```text
title = Harbor Lane Provisions - Annual Report 2025
subject = Annual report and financial summary
keywords = annual report, wholesale, Autumn Harvest, revenue, logistics
author = Amara Osei
application = Aspose Pty Ltd.
last-saved-time = 2/16/2026 12:00:00 PM
```
[Download full output](/parser/python-net/_output_files/getting-started/quick-start-guide/extract_metadata/extract-metadata.txt)
{{< /tab >}}
{{< /tabs >}}

### Run the App

{{< tabs "run_the_app_extract_metadata">}}
{{< tab "Windows" >}}
```ps
py extract_metadata.py
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 extract_metadata.py
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 extract_metadata.py
```
{{< /tab >}}
{{< /tabs >}}

### Explanation
- `parser.get_metadata()`: Returns the document's metadata items, or `None` for a format without metadata extraction.
- `item.name` / `item.value`: The name and the value of each property, such as `title` or `author`.

## Example 3: Extract tables

Sometimes you need the data of a document rather than its text. `get_tables()` returns each table with its rows and cells.

{{< tabs "demo_app_extract_tables">}}
{{< tab "extract_tables.py" >}}
```python
import os
from groupdocs.parser import Parser, License

def extract_tables():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Parser.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Open the document and print each table row by row
    with Parser("./sample.pdf") as parser:
        tables = parser.get_tables()

        if tables is None:
            print("Table extraction isn't supported for this format")
            return

        for table in tables:
            print("Table on page", table.page.index + 1)
            for row in range(table.row_count):
                cells = [table[row, column].text for column in range(table.column_count)]
                print(" | ".join(cells))

if __name__ == "__main__":
    extract_tables()
```
{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
`sample.pdf` is the sample file used in this example. Click [here](/parser/python-net/_sample_files/getting-started/quick-start-guide/sample.pdf) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "extract-tables.txt" >}}  
```text
Table on page 1
Metric | 2024 | 2025 | Change
Net revenue | $18.6 m | $21.4 m | +15.1%
Gross margin | 31.2% | 32.8% | +1.6 pts
Orders shipped | 48,900 | 55,300 | +13.1%
Wholesale customers | 1,140 | 1,265 | +11.0%
On-time delivery | 96.1% | 97.4% | +1.3 pts
Table on page 3
Category | Revenue | Share | Growth
Coffee & Tea | $5.9 m | 27.6% | +18.4%
[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/getting-started/quick-start-guide/extract_tables/extract-tables.txt)
{{< /tab >}}
{{< /tabs >}}

### Run the App

{{< tabs "run_the_app_extract_tables">}}
{{< tab "Windows" >}}
```ps
py extract_tables.py
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 extract_tables.py
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 extract_tables.py
```
{{< /tab >}}
{{< /tabs >}}

### Explanation
- `parser.get_tables()`: Returns the tables of the document, or `None` for a format without table extraction.
- `table.page.index`: The zero-based index of the page the table is on.
- `table[row, column].text`: The text of one cell; `row_count` and `column_count` give the table's size.

## Next Steps

After completing the basics, explore additional resources to enhance your usage:
- [Get document info]({{< ref "parser/python-net/developer-guide/basic-usage/get-document-info.md" >}}): Read the file type, page count and size of a document.
- [Extract images]({{< ref "parser/python-net/developer-guide/basic-usage/extract-images-from-documents.md" >}}): Extract the images of a document and save them to files.
- [Extract text from documents]({{< ref "parser/python-net/developer-guide/basic-usage/extract-text-from-documents.md" >}}): Extract the text of one page, and choose between the accurate and the raw mode.
- [Supported Document Formats]({{< ref "parser/python-net/getting-started/supported-document-formats.md" >}}): Review the full list of supported file types, or [list them in code]({{< ref "parser/python-net/developer-guide/basic-usage/get-supported-file-formats.md" >}}).
- [Licensing and Evaluation]({{< ref "parser/python-net/getting-started/licensing-and-subscription.md" >}}): Check details on licensing and evaluation.
- [Developer Guide]({{< ref "parser/python-net/developer-guide/_index.md" >}}): Dive into tables, images, annotations, templates and containers across every supported format.
- [Technical Support]({{< ref "parser/python-net/technical-support.md" >}}): Contact support for assistance if you encounter issues.
