---
id: mcp-tool-get-document-info
url: parser/mcp/tools-reference/get-document-info
title: get_document_info
weight: 6
description: "The get_document_info MCP tool returns file type, page count, and size as JSON without extracting anything."
keywords: get_document_info MCP, page count before extraction, inspect document agent, document triage MCP
productName: GroupDocs.Parser MCP Server
generated: true
serverVersion: 26.9.0
toc: True
---

`get_document_info` returns the file type, page count, and size — the cheap triage call before an extraction run. Example prompt: *"How many pages does invoice.pdf have, and what format is it?"*

**Tool description (as the AI agent sees it):**

> Returns basic information about a document — file type, page count, size — as JSON, without modifying the file. Supports PDF, DOCX, XLSX, PPTX, PNG, JPG, HTML, EPUB, MSG, EML, and 50+ more document formats. Call this tool whenever the user asks to get document info, check a file's details, or inspect it before extracting text / metadata / images / tables. Do NOT pre-check whether files exist — just pass the filename the user provided. Returns a JSON object with fields `fileName`, `fileType` (extension), `fileTypeName` (engine-reported format name), `pageCount`, and `size`. On failure, the response text starts with 'Document-info lookup failed for' followed by the underlying exception type, message, and inner-exception chain.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes |  — [FileInput shape]({{< ref "parser/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `password` | string | no | Password for protected documents |

## Example call

```json
{
  "name": "get_document_info",
  "arguments": {
    "file": {
      "filePath": "invoice.pdf"
    }
  }
}
```

## Result

A JSON object with the file type, page count, and size.

Use it to decide **how** to extract: a 400-page document wants page-by-page text extraction rather than one truncated response, and a 2 MB "PDF" that yields no text is probably a scan.

On failure the text starts with `Document-info lookup failed for`, followed by the exception type and message.

## Example prompts

* *"How many pages does this document have?"*
* *"What format and size is this file?"*
* *"Check these before I extract from all of them."*
