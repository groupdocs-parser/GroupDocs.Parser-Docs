---
id: mcp-tool-extract-metadata
url: parser/mcp/tools-reference/extract-metadata
title: extract_metadata
weight: 3
description: "The extract_metadata MCP tool extracts author, title, dates, custom properties, EXIF, XMP and IPTC from documents and images as JSON."
keywords: extract_metadata MCP tool, document properties extraction, EXIF extraction AI agent, XMP IPTC MCP
productName: GroupDocs.Parser MCP Server
generated: true
serverVersion: 26.9.0
toc: True
---

`extract_metadata` returns the document's properties as JSON — author, title, creation date, page count, custom properties, and image packages such as EXIF, XMP and IPTC. Example prompt: *"Who created this file and when?"*

**Tool description (as the AI agent sees it):**

> Extracts metadata from a document file (author, title, creation date, page count, custom properties, EXIF, XMP, IPTC) and returns it as JSON. Supports PDF, DOCX, XLSX, PPTX, JPEG, PNG, TIFF, MP3, MP4, and 50+ more document and image formats. Call this tool immediately whenever the user asks to extract metadata or get document properties from a file. Do NOT pre-check whether files exist — just pass the filename the user provided. Returns a JSON object whose keys are metadata field names (e.g. 'Author', 'Title', 'CreatedDate') and values are the corresponding string values. On failure, the response text starts with 'Metadata extraction failed for' followed by the underlying exception type, message, and inner-exception chain.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes |  — [FileInput shape]({{< ref "parser/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `password` | string | no | Password for protected documents |

## Example call

```json
{
  "name": "extract_metadata",
  "arguments": {
    "file": {
      "filePath": "invoice.pdf"
    }
  }
}
```

## Result

A JSON object of metadata properties, including image-specific packages where they exist.

This is **read-only**. To *change* or *remove* metadata — strip an author before sharing, clear EXIF GPS — use the [GroupDocs.Metadata MCP server](/metadata/mcp/), which is built for editing.

On failure the text starts with `Metadata extraction failed for`, followed by the exception type and message.

## Example prompts

* *"Who created this document and when?"*
* *"Show me the EXIF data from this photo."*
* *"List the custom properties on these files."*
