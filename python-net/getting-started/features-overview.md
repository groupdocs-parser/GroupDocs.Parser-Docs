---
id: features-overview
url: parser/python-net/features-overview
title: Features Overview
weight: 1
description: "This article describes the main functions of GroupDocs.Parser for Python via .NET. Extracting text, images, metadata, tables, and structured data from documents with template-based parsing support."
keywords: text extraction, parse data, extract metadata, extract images, template parsing
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
---

{{< alert style="info" >}}GroupDocs.Parser is a feature-rich document data parsing API. Here you may find description of the most important features.{{< /alert >}}

## Parse Data from Documents

GroupDocs.Parser allows to parse documents by user-defined templates.

It is easy to create a template with data field definitions and table definitions. Then it's easy to use the template (just pass the Template object to `parse_by_template` method) and extract data such as prices, invoices, tables from your typical documents.

## Extract Text

GroupDocs.Parser provides several text extraction methods that cover various text retrieval scenarios.

*   Extract plain text from any of the supported documents
*   Extract HTML or Markdown formatted text for a fast preview
*   Extract structured text
*   Extract text areas with coordinates, text style and other info
*   Search text by a keyword or regular expression; get text around the found word

Below different text extraction aspects are described:

### Accurate Text Extraction Mode

One of the most demanded features is accurate text extraction. GroupDocs.Parser allows to easily implement it using simple `get_text` method.

### Raw Text Extraction Mode

GroupDocs.Parser provides a way to increase text extraction performance with **Raw** text extraction mode for some formats. The text doesn't look so accurate, but performance is higher.

This feature is useful in those text extraction scenarios when text quality may not be the best, but performance is critical.

### Extract Formatted Text

In addition to standard text extraction modes, GroupDocs.Parser API provides a method `get_formatted_text` to extract formatted text for those cases when simple plain text is not enough and you may need to keep formatting like text style, table layout etc.

At this moment the following formats are supported:

*   Plain Text
*   Markdown
*   HTML

#### Plain Text

With Plain Text mode GroupDocs.Parser performs formatting in plain text making extracted text look closer to original. This is achieved due to special text positioning, box-drawing characters etc.

#### Markdown

This mode is useful when you need to export the extracted text to any system that supports [Markdown](https://en.wikipedia.org/wiki/Markdown)-formatted text.

At this moment the following formatting are supported:

*   Bold text
*   Italic text
*   Hyperlinks
*   Headings
*   Numbering and bullets lists
*   Tables

#### HTML

GroupDocs.Parser also supports HTML formatting.

Following HTML tags are now supported when extracting text with this formatting mode:

| Tag | Description |
| --- | --- |
| `<p>` | Paragraph is surrounded by `<p>` tag |
| `<a>` | Hyperlinks |
| `<b>` | Text with Bold font is surrounded by `<b>` tag |
| `<i>` | Text with Italic font is surrounded by `<i>` tag |
| `<h1>` – `<h6>` | If the heading has 'Heading X' style, it's surrounded by `<hX>` tag |
| `<ol>`/`<ul>` | Numbering and bullets lists |
| `<table>` | Tables |

### Extract Structured Text

Many document formats do not contain only text. Usually, the text is organized into paragraphs divided into parts with headers. Also, the text can contain hyperlinks, lists, tables. For this scenario, GroupDocs.Parser provides structured text extraction with the ability to extract text with its structure. This feature is easy to use - you simply call `get_structure` method that returns XML with structured text.

### Extract Text Areas

GroupDocs.Parser provides API that allows to extract text areas with coordinates and text style.

This feature allows to implement advanced scenarios related to text analytics in your applications. Just call `get_text_areas` method and you will get all text area objects.

### Search Text in Documents

GroupDocs.Parser allows to perform search over loaded document using keywords or regular expressions. Use the `search` method and then loop through the collection of search results.

## Extract Metadata

GroupDocs.Parser provides API that allows to extract metadata from supported document formats with simple `get_metadata` method call.

## Extract Images

GroupDocs.Parser supports image extraction from documents. You may call `get_images` method that returns all info about document images and allows to save them.

## Extract Tables

GroupDocs.Parser allows to extract tables from documents preserving their structure. You can use `get_tables` method to extract tables from the entire document or specific pages.

## Extract Data from Attachments and ZIP Archives

GroupDocs.Parser allows to extract data (text, images, other supported extraction methods) from formats that contain other documents like ZIP archives, PDF portfolios, emails, OST containers.

You can simply call `get_container` method and work with extracted attached or archived documents as with usual document files.

## Extract Document Information

GroupDocs.Parser provides the ability to get basic document information such as file type, page count, and size. This can be done using the `get_document_info` method.

## Extract Table of Contents

GroupDocs.Parser allows to extract table of contents from some document formats. To do it, you may call `get_toc` method.
