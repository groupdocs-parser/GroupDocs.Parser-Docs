---
id: features-overview
url: parser/python-net/features-overview
title: Features Overview
linkTitle: Features Overview
weight: 1
description: "GroupDocs.Parser for Python via .NET extracts text, formatted text, tables, images, hyperlinks, metadata and annotations through a single, format-independent API, and parses documents by templates."
keywords: text extraction, formatted text, markdown, html, extract tables, extract images, extract metadata, annotations, template parsing, OCR, python
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
---

GroupDocs.Parser for Python via .NET extracts data from a wide range of [supported document formats]({{< ref "parser/python-net/getting-started/supported-document-formats.md" >}}). Every operation follows the same workflow: open a document with `Parser`, call an extraction method such as `get_text()`, `get_tables()` or `get_metadata()`, and read the result. A method returns `None` when the document's format does not support that operation, so one code path serves every format.

## Text extraction

Extract the plain text of a whole document or of one page. The accurate mode keeps the reading order and layout; the raw mode is faster and fits pipelines where speed matters more than layout. Scanned pages and images are read with OCR. See [Extract text from documents]({{< ref "parser/python-net/developer-guide/basic-usage/extract-text-from-documents.md" >}}), [Extract text in Accurate mode]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-in-accurate-mode.md" >}}), and [Extract text in Raw mode]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-in-raw-mode.md" >}}).

## Formatted text

`get_formatted_text()` returns HTML, Markdown or plain text that keeps the document's headings, numbered and bulleted lists, tables, hyperlinks, and bold and italic text, ready for an HTML preview or a Markdown pipeline. See [Working with Text]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/working-with-text.md" >}}).

## Search, structure, and text areas

Search a document by keyword or regular expression and get the text around each hit, read its structure (paragraphs, headings, lists, tables and hyperlinks) as XML, or get its text areas with their coordinates and text style. See [Search text]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/search-text.md" >}}), [Extract highlights]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-highlights.md" >}}), [Extract text structure]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-structure.md" >}}), and [Extract text areas]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-areas.md" >}}).

## Tables

Extract the tables of PDF, Word, spreadsheet and presentation pages with their rows and cells, from a whole document or one page. See [Extract tables from document]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document.md" >}}) and [Extract tables from document page]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document-page.md" >}}).

## Images and hyperlinks

Extract the images of a document, a page or a rectangular page area, and save them to files in their own format or another. Hyperlinks are extracted the same way. See [Extract Images from Documents]({{< ref "parser/python-net/developer-guide/basic-usage/extract-images-from-documents.md" >}}), [Extract images to files]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-images/extract-images-to-files.md" >}}), and [Extract hyperlinks from document]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document.md" >}}).

## Metadata and document information

Read document properties such as title, author and dates with `get_metadata()`, the table of contents with `get_toc()`, and the file type, page count and size with `get_document_info()`. List every format the library reads with `FileType.get_supported_file_types()`. See [Extract Metadata from Documents]({{< ref "parser/python-net/developer-guide/basic-usage/extract-metadata-from-documents.md" >}}), [Get document info]({{< ref "parser/python-net/developer-guide/basic-usage/get-document-info.md" >}}), and [Get supported file formats]({{< ref "parser/python-net/developer-guide/basic-usage/get-supported-file-formats.md" >}}).

## Annotations

Read the comments and notes reviewers leave in PDF, Word and PowerPoint files. Call `get_annotations()` for the whole document, or `get_annotations(page_index)` for one page; each annotation carries its text, type, page, author and timestamp. To get the annotations' text inside the extracted text, set `TextOptions.include_annotations`. See [Working with annotations]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-annotations/_index.md" >}}).

## Templates

Describe the fields and tables of documents that share a layout, such as invoices, once in a template, then pass it to `parse_by_template()` to read their values from every such document. See [Parse Data from Documents]({{< ref "parser/python-net/developer-guide/basic-usage/parse-data-from-documents.md" >}}) and [Working with Templates]({{< ref "parser/python-net/developer-guide/advanced-usage/template-based-parsing/working-with-templates.md" >}}).

## Archives, attachments, and portfolios

`get_container()` lists the documents inside ZIP, RAR, 7Z and TAR archives, email attachments, Outlook data files and PDF portfolios, and each item opens with a `Parser` of its own. See [Extract Data from Attachments and ZIP Archives]({{< ref "parser/python-net/developer-guide/basic-usage/extract-data-from-attachments-and-zip-archives.md" >}}) and [Working with Containers]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-archives/working-with-containers.md" >}}).

## Loading documents

Open a document from a file path or a binary stream, open password-protected documents, and tell the library a document's format when its name does not. See [Load file from stream]({{< ref "parser/python-net/developer-guide/advanced-usage/loading/load-file-from-stream.md" >}}) and [Load password-protected documents]({{< ref "parser/python-net/developer-guide/advanced-usage/loading/load-password-protected-documents.md" >}}).

## Logging and external resources

Pass a Python function, or an object with `trace`, `warning` and `error` methods, to `ParserSettings` to receive the engine's log messages. A resource handler decides which images an HTML or MHTML document may load. See [Logging]({{< ref "parser/python-net/developer-guide/advanced-usage/logging.md" >}}) and [Handle loading of external resources]({{< ref "parser/python-net/developer-guide/advanced-usage/loading/handle-loading-of-external-resources.md" >}}).

## Command line

The package installs the `groupdocs-parser` command, which extracts text, document info, metadata, images and annotations without writing Python code. See [Command Line Interface]({{< ref "parser/python-net/developer-guide/command-line-interface.md" >}}).

## On-premise

GroupDocs.Parser for Python via .NET runs entirely on your own infrastructure — your documents never leave your environment. The package is a self-contained wheel that bundles everything it needs, so no Microsoft Office, OpenOffice, or separate runtime has to be installed. See [System Requirements]({{< ref "parser/python-net/getting-started/system-requirements.md" >}}) for the supported platforms and native dependencies.
