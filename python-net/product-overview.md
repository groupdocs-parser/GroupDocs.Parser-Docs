---
id: product-overview
url: parser/python-net/product-overview
title: GroupDocs.Parser for Python via .NET Overview
linkTitle: Product overview
weight: 1
description: "GroupDocs.Parser for Python via .NET extracts text, formatted text, tables, images, metadata, hyperlinks and annotations from PDF, Word, Excel, PowerPoint, email, eBook, archive and image files, and parses documents by templates, on-premise, with no Microsoft Office required."
keywords: parse documents, extract text, extract tables, extract images, extract metadata, annotations, markdown, OCR, templates, PDF, DOCX, XLSX, PPTX, python
productName: GroupDocs.Parser for Python via .NET
toc: True
aliases:
    - /parser/python-net/groupdocs-parser-overview/
structuredData:
  showOrganization: True
  application:
    name: Document Parser Tool
    description: The product extracts text, images, metadata, tables, and structured data from PDF, Word, Excel, PowerPoint and 65+ other file formats. Parser API also supports template-based extraction, document information retrieval, and container processing.
    productCode: parser
    productPlatform: python-net
---

## What is GroupDocs.Parser?

GroupDocs.Parser for Python via .NET is a native Python library that extracts **text, formatted text, tables, images, metadata, hyperlinks and annotations** from PDF, Word, Excel, PowerPoint, OpenDocument, email, eBook, archive and image files, 65+ formats in all. It also searches text, reads document structure, recognizes text in scanned pages (OCR), and pulls fields out of invoices and forms with templates. It runs entirely on-premise, requires no Microsoft Office installation, and ships as a pre-built wheel for Windows, Linux, and macOS.

Typical uses include:

- **AI and search pipelines** — feed the plain or Markdown text of PDFs, Office files and emails into a search index, a RAG pipeline or an LLM.
- **Data extraction** — pull the tables out of reports into rows and cells, and read invoice numbers, dates and totals with a template.
- **Content migration** — export the text, images and metadata of a document archive to another system.
- **Document review** — collect the comments reviewers left in Word, PDF and PowerPoint files, with their authors and pages.
- **Inventory and triage** — read file types, page counts and document properties across a folder, archives and email attachments included.

## Key Capabilities

| Capability | Description |
|---|---|
| **Text extraction** | [Extract the text]({{< ref "parser/python-net/developer-guide/basic-usage/extract-text-from-documents.md" >}}) of a document or one page, in accurate or fast raw mode, with OCR for scanned pages and images. |
| **Formatted text** | [Get HTML, Markdown or plain text]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/working-with-text.md" >}}) that keeps headings, lists, tables and emphasis. |
| **Search** | [Search the text]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/search-text.md" >}}) by keyword or regular expression, with the text around each hit. |
| **Text structure and areas** | [Read the document's structure as XML]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-structure.md" >}}), and text areas with their coordinates and style. |
| **Tables** | [Extract tables]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document.md" >}}) from PDF, Word, spreadsheet and presentation pages, cell by cell. |
| **Images** | [Extract the images]({{< ref "parser/python-net/developer-guide/basic-usage/extract-images-from-documents.md" >}}) of a document, a page or a page area, and save them in their own format or another. |
| **Hyperlinks** | [Extract the hyperlinks]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document.md" >}}) of a document, a page or a page area. |
| **Metadata** | [Read document properties]({{< ref "parser/python-net/developer-guide/basic-usage/extract-metadata-from-documents.md" >}}) such as title, author and dates. |
| **Annotations** | [Read comments and notes]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-annotations/extract-annotations-from-document.md" >}}) with their author, time and page. |
| **Templates** | [Parse documents by template]({{< ref "parser/python-net/developer-guide/basic-usage/parse-data-from-documents.md" >}}) to read fields and tables from documents that share a layout, such as invoices. |
| **Containers** | [Work with ZIP archives, email attachments and PDF portfolios]({{< ref "parser/python-net/developer-guide/basic-usage/extract-data-from-attachments-and-zip-archives.md" >}}) as with ordinary documents. |
| **Document inspection** | [Read the file type, page count and size]({{< ref "parser/python-net/developer-guide/basic-usage/get-document-info.md" >}}) of a document without parsing it. |
| **Streams** | [Load documents from file-like objects]({{< ref "parser/python-net/developer-guide/advanced-usage/loading/load-file-from-stream.md" >}}), handy for cloud blobs and HTTP bodies. |
| **Logging and callbacks** | [Receive the engine's log messages]({{< ref "parser/python-net/developer-guide/advanced-usage/logging.md" >}}) and decide which external resources a document may load, with Python functions or objects. |
| **Command line** | [Extract text, metadata, images and annotations]({{< ref "parser/python-net/developer-guide/command-line-interface.md" >}}) with the `groupdocs-parser` command, without writing Python code. |
| **On-premise** | No cloud calls, no Microsoft Office install, no network traffic. See [System Requirements]({{< ref "parser/python-net/getting-started/system-requirements.md" >}}). |

## Quick Example

Extract the text of a document with just a few lines of code:

{{< tabs "product-overview-extract-text" >}}
{{< tab "quick_example.py" >}}
```python
from groupdocs.parser import Parser

def quick_example():
    # Open the document and extract its text
    with Parser("./sample.pdf") as parser:
        text = parser.get_text()

        # get_text() returns None for a format without text extraction
        if text is None:
            print("Text extraction isn't supported for this format")
        else:
            print(text)

if __name__ == "__main__":
    quick_example()
```
{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
`sample.pdf` is the sample file used in this example. Click [here](/parser/python-net/_sample_files/product-overview/sample.pdf) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "quick.txt" >}}  
```text
Investor relations


Annual Report 2025  |  Port Ellery
Annual Report 2025


1. Overview
Harbor Lane Provisions supplies specialty food to 1,265 independent grocers, cafes and restaurants from its
Quayside warehouse in Port Ellery. In 2025 net revenue grew 15.1% to $21.4 million, driven by the Autumn
[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/product-overview/quick_example/quick.txt)
{{< /tab >}}
{{< /tabs >}}

To keep the headings, lists and tables of a document, extract its text as Markdown:

{{< tabs "product-overview-extract-markdown" >}}
{{< tab "extract_markdown.py" >}}
```python
from groupdocs.parser import Parser
from groupdocs.parser.options import FormattedTextMode, FormattedTextOptions

def extract_markdown():
    # Open the document and extract its text as Markdown
    with Parser("./sample.docx") as parser:
        options = FormattedTextOptions(FormattedTextMode.MARKDOWN)
        markdown = parser.get_formatted_text(options)

        if markdown is None:
            print("Formatted text extraction isn't supported for this format")
        else:
            print(markdown)

if __name__ == "__main__":
    extract_markdown()
```
{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
`sample.docx` is the sample file used in this example. Click [here](/parser/python-net/_sample_files/product-overview/sample.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "extract-markdown.txt" >}}  
```text
**Supply Agreement**

Harbor Lane Provisions and Northwind Coffee Roasters

# **1. Parties**

**Buyer:** Harbor Lane Provisions, 18 Quayside Road, Port Ellery, represented by Lena Park, Procurement Lead.

**Supplier:** Northwind Coffee Roasters, 4 Roastery Lane, Millbrook, represented by Alex Chen.

[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/product-overview/extract_markdown/extract-markdown.txt)
{{< /tab >}}
{{< /tabs >}}

## Where to next

1. **Install the package** — [Installation]({{< ref "parser/python-net/getting-started/installation.md" >}}) walks through PyPI and offline wheel installation for Windows, Linux, and macOS.
2. **Extract your first text** — [Quick Start Guide]({{< ref "parser/python-net/getting-started/quick-start-guide.md" >}}) extracts text, metadata and tables in five minutes.
3. **Explore the examples** — [How to Run Examples]({{< ref "parser/python-net/getting-started/how-to-run-examples.md" >}}) clones the runnable repository and runs every documented scenario locally or in Docker.
4. **Use it in depth** — the [Developer Guide]({{< ref "parser/python-net/developer-guide/_index.md" >}}) covers text, tables, images, metadata, annotations, templates and containers in every supported format.
5. **Licensing** — [Licensing and Evaluation]({{< ref "parser/python-net/getting-started/licensing-and-subscription.md" >}}) explains evaluation mode and how to apply a license.
