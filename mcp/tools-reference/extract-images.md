---
id: mcp-tool-extract-images
url: parser/mcp/tools-reference/extract-images
title: extract_images
weight: 4
description: "The extract_images MCP tool extracts embedded images from a document and saves them to storage as separate image files."
keywords: extract_images MCP tool, extract images from PDF AI, save embedded images document, get pictures from docx
productName: GroupDocs.Parser MCP Server
generated: true
serverVersion: 26.9.0
toc: True
---

`extract_images` saves every embedded image in a document to your storage folder as separate files, named `<basename>_image<N>.<ext>`. Restrict to one page with `page`. Example prompt: *"Pull the images out of this deck."*

**Tool description (as the AI agent sees it):**

> Extracts all images from a document and saves them to storage as separate image files. Supports PDF, DOCX, XLSX, PPTX, HTML, EPUB, and 30+ more formats with embedded images. Call this tool immediately whenever the user asks to extract images, get images, or save images from a document. Do NOT pre-check whether files exist — just pass the filename the user provided. Returns a list of saved-path messages, one per extracted image (named '<basename>_image<N>.<ext>'). On failure, the response text starts with 'Image extraction failed for' followed by the underlying exception type, message, and inner-exception chain.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes |  — [FileInput shape]({{< ref "parser/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `password` | string | no | Password for protected documents |
| `page` | integer | no | Page number to extract images from (1-based). Omit for all pages. |

## Example call

```json
{
  "name": "extract_images",
  "arguments": {
    "file": {
      "filePath": "deck.pptx"
    },
    "page": 3
  }
}
```

## Result

A list of saved-path messages, one per extracted image.

The images are written to disk rather than returned inline, which keeps the response small and makes the results usable by everything else on your machine. A document with many images produces many files — restrict by `page` when you only need one.

On failure the text starts with `Image extraction failed for`, followed by the exception type and message.

## Example prompts

* *"Pull the images out of this presentation."*
* *"Extract the figures from page 3 of the report."*
* *"Save every embedded picture from these documents."*
