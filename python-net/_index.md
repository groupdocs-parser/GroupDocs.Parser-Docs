---
id: home
url: parser/python-net
title: GroupDocs.Parser for Python via .NET
weight: 1
description: "Native Python library that extracts text, formatted text, tables, images, metadata, hyperlinks and annotations from PDF, Word, Excel, PowerPoint, email, eBook, archive and image files, 65+ formats, on Windows, Linux, and macOS. No Microsoft Office or OpenOffice required."
keywords: GroupDocs.Parser, Python via .NET, parse documents, extract text, extract tables, extract images, extract metadata, markdown, OCR, templates, PDF, Word, Excel, PowerPoint, email, on-premise, Windows, Linux, macOS, python
productName: GroupDocs.Parser for Python via .NET
hideChildren: true
toc: True
structuredData:
    showOrganization: true
---

<img src="/logo/128x128/groupdocs-parser-python.png" alt="groupdocs-parser-python-net-home" align="left" style="width:110px; margin: 0 30px 30px 0"/>

<img src="https://img.shields.io/pypi/v/groupdocs-parser-net?label=GroupDocs.Parser%20PyPI" alt="PyPI package">
<img src="https://img.shields.io/pypi/dm/groupdocs-parser-net?label=pypi%20downloads" alt="PyPI downloads">

{{< button style="primary" link="https://releases.groupdocs.com/parser/python-net/release-notes/" >}} <svg class="gdoc-icon gdoc-product-doc__btn-icon"><use xlink:href="/img/groupdocs-stack.svg#document"></use></svg> Release notes {{< /button >}}
{{< button style="primary" link="https://releases.groupdocs.com/parser/python-net/" >}} {{< icon "gdoc_download" >}} Download the wheels {{< /button >}}
{{< button style="primary" link="https://products.groupdocs.app/parser/family" >}} <svg class="gdoc-icon gdoc-product-doc__btn-icon"><use xlink:href="/img/groupdocs-stack.svg#app"></use></svg> Online app {{< /button >}}

[GroupDocs.Parser for Python via .NET](https://products.groupdocs.com/parser/python-net/) is a document parsing API: extract the text of a document or a page, get it as HTML or Markdown, pull out its tables, images, hyperlinks, metadata and annotations, search it, and read fields from invoices and forms with templates. It works with Word processing, spreadsheets, presentations, PDF, email, eBooks, markup, archives and images, 65+ formats, with no MS Office or OpenOffice installation required.

<div style="clear:left"></div>

{{< alert style="info" >}}
Version 26.10 is available on the [GroupDocs Releases website](https://releases.groupdocs.com/parser/python-net/) and is not on PyPI yet: until it is, `pip install groupdocs-parser-net` installs 25.12. See [Installation]({{< ref "parser/python-net/getting-started/installation.md" >}}).
{{< /alert >}}

## Quick example

```python
from groupdocs.parser import Parser

# Open a document and print its text
with Parser("document.pdf") as parser:
    print(parser.get_text())
```

## Features

- **Text extraction**: The plain text of a document or one page, in accurate or fast raw mode, with OCR for scanned pages and images.
- **Formatted text**: HTML, Markdown or plain text that keeps headings, lists, tables and emphasis.
- **Tables, images and hyperlinks**: From whole documents, single pages or rectangular page areas.
- **Metadata, annotations and structure**: Document properties, reviewers' comments with their authors and pages, the table of contents, and the document's structure as XML.
- **Search**: Keyword and regular-expression search with the text around each hit.
- **Templates**: Read fields and tables from documents that share a layout, such as invoices.
- **Containers**: Process the documents inside ZIP, RAR, 7Z and TAR archives, email attachments and PDF portfolios.
- **Command line**: The `groupdocs-parser` command extracts text, metadata, images and annotations without writing Python code.

## Supported File Formats

GroupDocs.Parser reads 65+ file formats. For a complete list, see the [full list of supported formats]({{< ref "parser/python-net/getting-started/supported-document-formats.md" >}}).

- **Word Processing** (DOC, DOCX, DOCM, DOT, DOTX, DOTM, ODT, OTT, RTF, TXT)
- **Spreadsheets** (XLS, XLSX, XLSM, XLSB, XLT, XLTX, XLTM, XLA, XLAM, ODS, OTS, CSV, TSV)
- **Presentations** (PPT, PPTX, PPTM, PPS, PPSX, PPSM, POT, POTX, POTM, ODP, OTP)
- **Fixed-Layout** (PDF)
- **Email** (EML, EMLX, MSG, PST, OST)
- **eBooks** (EPUB, MOBI, AZW3, FB2, CHM)
- **Markup** (HTML, XHTML, MHTML, XML, MD)
- **Images** (JPG, JPEG, PNG, BMP, GIF, TIF, TIFF, JP2)
- **Archives** (ZIP, RAR, 7Z, TAR, GZ, BZ2)
- **OneNote** (ONE)

## Getting Started

To get started, refer to the [System Requirements]({{< ref "parser/python-net/getting-started/system-requirements.md" >}}), [Supported Document Formats]({{< ref "parser/python-net/getting-started/supported-document-formats.md" >}}), [Installation]({{< ref "parser/python-net/getting-started/installation.md" >}}), and [Quick Start Guide]({{< ref "parser/python-net/getting-started/quick-start-guide.md" >}}) sections for setup instructions.

## Developer Guide

For practical code examples and explanations of the open–extract workflow, see the [Developer Guide]({{< ref "parser/python-net/developer-guide/_index.md" >}}) section. It covers extracting text, formatted text, tables, images, hyperlinks, metadata and annotations, searching text, parsing by templates, containers, loading options, logging and the command line.

## AI Agents and LLM Integration

The package ships an `AGENTS.md` reference for AI coding assistants, and the documentation is available to them through an MCP server and as machine-readable text. See [AI agents and LLM integration]({{< ref "parser/python-net/agents-and-llm-integration.md" >}}).

## Technical Support

If you experience any issues or have suggestions, please refer to the [Technical Support]({{< ref "parser/python-net/technical-support.md" >}}) topic. This topic provides multiple support channels tailored to your needs.
