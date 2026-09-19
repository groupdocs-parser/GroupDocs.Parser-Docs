---
id: mcp-tool-extract-text
url: parser/mcp/tools-reference/extract-text
title: extract_text
weight: 1
description: "The extract_text MCP tool extracts plain text from PDF, Word, Excel, PowerPoint, email and 50+ other formats — the whole document or a single page."
keywords: extract_text MCP tool, extract text from PDF AI agent, read document content MCP, get text from docx
productName: GroupDocs.Parser MCP Server
generated: true
serverVersion: 26.9.0
toc: True
---

`extract_text` returns the plain text of a document — all of it, or one page with `page`. The starting point for *"what does this say?"* across PDF, Office, email, e-book and 50+ other formats. Example prompt: *"Extract the text from page 2 of contract.docx."*

**Tool description (as the AI agent sees it):**

> Extracts plain text from a document file. Supports PDF, DOCX, XLSX, PPTX, TXT, HTML, CSV, EML, MSG, RTF, ODT, EPUB, and 50+ more formats. Call this tool immediately whenever the user asks to extract text, read text, or get the content of a document. Do NOT pre-check whether files exist — just pass the filename the user provided. Returns the document's plain text (truncated with a marker if it exceeds the configured budget). On failure, the response text starts with 'Text extraction failed for' followed by the underlying exception type, message, and inner-exception chain.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes |  — [FileInput shape]({{< ref "parser/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `password` | string | no | Password for protected documents |
| `page` | integer | no | Page number to extract from (1-based). Omit for all pages. |

## Example call

```json
{
  "name": "extract_text",
  "arguments": {
    "file": {
      "filePath": "contract.docx"
    },
    "page": 2
  }
}
```

## Result

The document's plain text. **Very large outputs are truncated** with a marker showing where it stopped — so ask for a specific `page` when you need a complete read of a long document, and loop over pages rather than requesting everything at once.

On a scanned document this returns little or nothing: there is no text layer to read, and no OCR step. That is a true answer about the file, not a failure — check with [`get_document_info`]({{< ref "parser/mcp/tools-reference/get-document-info.md" >}}) and, for codes on the page, [`extract_barcodes`]({{< ref "parser/mcp/tools-reference/extract-barcodes.md" >}}).

On failure the text starts with `Text extraction failed for`, followed by the exception type and message.

## Example prompts

* *"Extract the text from page 2 of contract.docx."*
* *"What does this email say?"*
* *"Pull the text out of these PDFs and summarize each one."*
* *"Get the content of page 12 — I only need that section."*

See it used end-to-end: [Extract document data with AI agents]({{< ref "parser/mcp/use-cases/extract-document-data-with-ai-agents.md" >}}).
