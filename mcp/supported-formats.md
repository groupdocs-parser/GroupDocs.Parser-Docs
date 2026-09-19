---
id: mcp-supported-formats
url: parser/mcp/supported-formats
title: Supported formats
weight: 4
description: "The MCP server exposes the full GroupDocs.Parser engine: PDF, Office, email, e-book, image and 50+ more formats can be read through your AI agent."
keywords: MCP server supported formats, extract text pdf MCP, extract from email MCP, epub extraction, barcode formats MCP
productName: GroupDocs.Parser MCP Server
toc: True
---

The MCP server exposes the **full GroupDocs.Parser engine**: every format the .NET library can read — 50+ document, spreadsheet, presentation, email, e-book, and image formats — is available to your AI agent. The canonical matrix lives in the library documentation: [supported document formats]({{< ref "parser/net/getting-started/supported-document-formats.md" >}}).

What each family gives you:

* **PDF** — text, tables, images, metadata, and barcodes. The format most extraction requests arrive as.
* **DOCX / XLSX / PPTX** — the cleanest extractions, because structure is explicit in the source. Spreadsheet tables come out as tables.
* **Email (MSG, EML)** — message text and properties, which is how a lot of business context is stored.
* **E-books (EPUB) and HTML** — long-form text with structure.
* **Images (JPG, PNG, TIFF)** — metadata (EXIF, XMP, IPTC) and **barcode detection**; no text extraction, because there is no text layer.

**The one rule worth internalising:** [`extract_text`]({{< ref "parser/mcp/tools-reference/extract-text.md" >}}) reads a **text layer**. Born-digital documents have one; scans do not. On a scan, text extraction returns little or nothing while [`extract_barcodes`]({{< ref "parser/mcp/tools-reference/extract-barcodes.md" >}}) still works — the engine's models detect codes in the rasterized image. There is no OCR step in this server, so printed words in a scan stay pixels.
