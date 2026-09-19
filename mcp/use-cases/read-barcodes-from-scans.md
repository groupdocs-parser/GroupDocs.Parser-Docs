---
id: mcp-uc-read-barcodes-from-scans
url: parser/mcp/use-cases/read-barcodes-from-scans
title: How to read barcodes and QR codes from scanned documents
linkTitle: Read barcodes from scans
weight: 3
description: "Decode barcodes and QR codes from scanned documents and images with an AI agent over MCP, locally — including rasterized pages with no text layer."
keywords: read barcode from scanned PDF, QR code extraction AI agent, decode barcodes document MCP, tracking number from scan
productName: GroupDocs.Parser MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to read barcodes and QR codes from scanned documents"
        description: "Decode barcodes and QR codes from scanned documents and images with an AI agent over MCP, locally — including rasterized pages with no text layer."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Parser MCP server with Docker or dnx and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "Read the barcodes in these scanned delivery notes and list the values with the file they came from."
---

Scanned documents defeat text extraction: there is no text layer to read. Barcodes are the exception — the engine's models detect them **in the rasterized image**, so a scanned delivery note still yields its tracking number.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server, which ships as a Docker image only. Installation and client setup: [MCP server for .NET]({{< ref "parser/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## The prompt

> Read the barcodes in these scanned delivery notes and list the values with the file they came from.

[`extract_barcodes`]({{< ref "parser/mcp/tools-reference/extract-barcodes.md" >}}) returns the decoded value, the symbology (Code128, QR Code, PDF417, DataMatrix, EAN-13, EAN-8, UPC, Aztec and more), the page, and the position.

## Why this matters operationally

The decoded value is usually the **join key**: the tracking number that links a scan to a shipment, the order reference that links a signed form to a case, the asset tag that links a photograph to an inventory record. Once the agent has it, the document stops being an opaque image and becomes a row you can look up.

> For each scan, read the barcode and tell me which order it belongs to.

## Narrow by page

> Read the barcode on page 1 only.

Scanning every page of a long document for codes is work you can skip when you know where the label is.

## What this is not

* **Not OCR.** Printed words in a scan remain pixels. If you need the *text* of a scanned page, that needs an OCR step this server does not provide.
* **Not a trust boundary.** A decoded value is data supplied by whoever produced the document. Report it, look it up, cross-check it — do not let an agent act on it unreviewed.
* **Not guaranteed on poor scans.** Skewed, low-resolution, or damaged codes may not decode. An empty result is "not found", which on a bad scan is not the same as "not present".

## Combining with the rest

On a mixed batch — some born-digital, some scanned — let the agent branch:

> For each file: try text extraction. If it comes back empty, read the barcodes instead and tell me which files needed that.

The list of files that fell through to barcodes is, incidentally, an inventory of which parts of your archive are scans — useful in its own right when planning an ingestion project ([Markdown server](/markdown/mcp/) has the same blind spot for the same reason).
