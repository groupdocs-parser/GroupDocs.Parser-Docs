---
id: features-overview
url: parser/nodejs-java/features-overview
title: Features Overview
weight: 1
description: "Overview of the main GroupDocs.Parser for Node.js via Java features: parsing by template, text, metadata, image, attachment, form and table of contents extraction."
keywords: features, parse by template, extract text, extract metadata, extract images, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
{{< alert style="info" >}}GroupDocs.Parser is a feature-rich document data parsing API. This page describes its most important features.{{< /alert >}}

## Parse Document by Template

GroupDocs.Parser allows to parse documents by user-defined templates.

It is easy to create a template with data field and table definitions. Then pass the `Template` object to the `parseByTemplate(template)` method and extract data such as prices, invoice numbers and tables from your typical documents. See [Parse data from documents](/parser/nodejs-java/parse-data-from-documents/) for an example.

## Extract Text

GroupDocs.Parser provides several text extraction methods that cover various text retrieval scenarios:

*   Extract a plain text from any of the supported documents;
*   Extract HTML or Markdown formatted text for a fast preview;
*   Extract structured text;
*   Extract text areas with coordinates, text style and other info;
*   Search a text by a keyword or regular expression; get a text around the found word.

Below different text extraction aspects are described.

### Accurate Text Extraction Mode

One of the most demanded features is accurate text extraction. GroupDocs.Parser allows to easily implement it using the simple `getText()` method. See [Extract text from documents](/parser/nodejs-java/extract-text-from-documents/).

### Raw Text Extraction Mode

GroupDocs.Parser provides a way to increase text extraction performance with **Raw** text extraction mode for some formats. The text doesn't look so accurate, but performance is higher.

This feature is useful in those text extraction scenarios when text quality may not be the best, but performance is critical.

### Extract Formatted Text

In addition to standard text extraction modes, GroupDocs.Parser provides the `getFormattedText(options)` method to extract a formatted text for those cases when simple plain text is not enough and you need to keep formatting like text style, table layout etc. See [Extract formatted text from documents](/parser/nodejs-java/extract-formatted-text-from-documents/).

At this moment the following formats are supported:

*   Plain Text
*   Markdown
*   HTML

#### Plain Text

With Plain Text mode GroupDocs.Parser performs formatting in plain text making extracted text look closer to the original. This is achieved due to special text positioning, box-drawing characters etc.

#### Markdown

This mode is useful when you need to export the extracted text to any system that supports [Markdown](https://en.wikipedia.org/wiki/Markdown)-formatted text.

At this moment the following formatting is supported:

*   Bold text
*   Italic text
*   Hyperlinks
*   Headings
*   Numbering and bullets lists
*   Tables

#### HTML

GroupDocs.Parser also supports HTML formatting.

The following HTML tags are supported when extracting text with this formatting mode:

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

Many document formats do not contain only a text. Usually, the text is organized into paragraphs divided into parts with headers. Also, the text can contain hyperlinks, lists, tables. For this scenario, GroupDocs.Parser provides structured text extraction. Call the `getStructure()` method that returns an `org.w3c.dom.Document` object with structured text in XML form.

### Extract Text Areas

GroupDocs.Parser provides an API that allows to extract text areas with coordinates and text style.

This feature allows to implement advanced scenarios related to text analytics in your applications. Call the `getTextAreas()` method and you will get all text area objects.

### Search Text in Documents

GroupDocs.Parser allows to search over a loaded document using keywords or regular expressions. Call the `search(keyword)` method and then loop through the collection of search results.

## Extract Metadata

GroupDocs.Parser allows to extract metadata from supported document formats with a simple `getMetadata()` method call. See [Extract metadata from documents](/parser/nodejs-java/extract-metadata-from-documents/).

## Extract Images

GroupDocs.Parser supports image extraction from documents. The `getImages()` method returns all info about document images and allows to save them. See [Extract images from documents](/parser/nodejs-java/extract-images-from-documents/).

## Extract Containers and Attachments

GroupDocs.Parser allows to extract data (text, images and other supported extraction methods) from formats that contain other documents like ZIP archives, PDF portfolios, emails, OST containers.

Call the `getContainer()` method and work with extracted attached or archived documents as with usual document files. See [Extract data from attachments and ZIP archives](/parser/nodejs-java/extract-data-from-attachments-and-zip-archives/).

## Parse Form Data

GroupDocs.Parser allows to parse form data from PDF documents. Call the `parseForm()` method and iterate through extracted form fields. See [Extract data from PDF forms](/parser/nodejs-java/extract-data-from-pdf-forms/).

## Extract Table of Contents

GroupDocs.Parser allows to extract table of contents from some document formats. To do it, call the `getToc()` method. See [Extract table of contents](/parser/nodejs-java/extract-table-of-contents/).
