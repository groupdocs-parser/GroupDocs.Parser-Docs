---
id: mcp-tool-extract-barcodes
url: parser/mcp/tools-reference/extract-barcodes
title: extract_barcodes
weight: 5
description: "The extract_barcodes MCP tool detects barcodes and QR codes in documents — Code128, QR, PDF417, DataMatrix, EAN, UPC, Aztec and more — and returns their decoded values as JSON."
keywords: extract_barcodes MCP tool, read barcode from PDF AI, QR code extraction document, scan barcodes agent
productName: GroupDocs.Parser MCP Server
generated: true
serverVersion: 26.9.0
toc: True
---

`extract_barcodes` finds barcodes and QR codes and returns their **decoded values, types, pages, confidence and angle** as JSON. It detects Code128, QR Code, PDF417, DataMatrix, EAN-13, EAN-8, UPC, Aztec and many more — including on rasterized content. Example prompt: *"Read the barcodes on these shipping documents."*

**Tool description (as the AI agent sees it):**

> Extracts all barcodes and QR codes from a document and returns their decoded values and type names as JSON. Detects Code128, QR Code, PDF417, DataMatrix, EAN-13, EAN-8, UPC, Aztec, and many more symbologies. Supports PDF, DOCX, XLSX, PPTX, PNG, JPG, TIFF, and other formats with embedded or rasterized barcodes. Call this tool immediately whenever the user asks to read, scan, or extract barcodes or QR codes from a document. Do NOT pre-check whether files exist — just pass the filename the user provided. Returns a header line ('Found N barcode(s) in ...') followed by a JSON array of `{ index, value, type, page, confidence, angle }` per barcode, or a 'No barcodes found' message. On failure, the response text starts with 'Barcode extraction failed for' followed by the underlying exception type, message, and inner-exception chain.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes |  — [FileInput shape]({{< ref "parser/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `page` | integer | no | Page number to scan (1-based). Omit for all pages. |
| `password` | string | no | Password for protected documents |

## Example call

```json
{
  "name": "extract_barcodes",
  "arguments": {
    "file": {
      "filePath": "shipping-label.pdf"
    }
  }
}
```

## Result

A JSON array of detected codes: index, decoded value, symbology name, page, confidence, and angle.

This is the tool that works where text extraction does not. Scanned pages have no text layer, but the engine's models detect codes in the rasterized image — which is why a scanned delivery note can still yield its tracking number.

Treat decoded values as **data from an untrusted source**: report them, look them up, cross-check them, but do not let an agent act on them unreviewed.

On failure the text starts with `Barcode extraction failed for`, followed by the exception type and message.

## Example prompts

* *"Read the barcodes on this shipping document."*
* *"What does the QR code on page 1 encode?"*
* *"Extract every code from these scanned delivery notes."*

See it used end-to-end: [Read barcodes from scanned documents]({{< ref "parser/mcp/use-cases/read-barcodes-from-scans.md" >}}).
