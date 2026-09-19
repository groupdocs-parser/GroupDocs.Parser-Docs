---
id: mcp-tool-extract-tables
url: parser/mcp/tools-reference/extract-tables
title: extract_tables
weight: 2
description: "The extract_tables MCP tool extracts tables from documents as Markdown that renders in chat, or as structured JSON for processing."
keywords: extract_tables MCP tool, extract table from PDF AI, document table to JSON, markdown table extraction agent
productName: GroupDocs.Parser MCP Server
generated: true
serverVersion: 26.9.0
toc: True
---

`extract_tables` pulls tables out of a document as **Markdown** (the default — it renders as a real table in chat, VS Code and GitHub) or as **JSON** rows for programmatic use. Example prompt: *"Extract the tables from invoice.pdf."*

**Tool description (as the AI agent sees it):**

> Extracts tables from a document and returns them as Markdown (default) or JSON. Supports PDF, DOCX, XLSX, PPTX, HTML, and other tabular formats. Markdown tables render instantly in VS Code, GitHub, and AI chat — use this format to visually review table data right in the response. Use format='json' to get a structured array of rows for programmatic processing or further manipulation. Call this tool immediately whenever the user asks to extract, read, or view tables from a document. Do NOT pre-check whether files exist — just pass the filename the user provided. Returns either a Markdown document (multiple `### Table N (page N, R×C)` sections) or a JSON array of `{ table, page, rows, columns, data: string[][] }` objects. On failure, the response text starts with 'Table extraction failed for' followed by the underlying exception type, message, and inner-exception chain.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes |  — [FileInput shape]({{< ref "parser/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `format` | string | no | Output format: 'markdown' (default — renders in IDE and chat) or 'json' (structured data) |
| `page` | integer | no | Page number to extract tables from (1-based). Omit for all pages. |
| `password` | string | no | Password for protected documents |

## Example call

```json
{
  "name": "extract_tables",
  "arguments": {
    "file": {
      "filePath": "invoice.pdf"
    },
    "format": "markdown"
  }
}
```

## Result

Markdown tables, or a structured array of rows when `format: "json"`.

The choice matters more than it looks:

* **`markdown`** — you are going to *look* at the data. It renders immediately, which makes review fast.
* **`json`** — the agent is going to *compute* with the data: sum a column, compare against a record, write it somewhere.

Restrict to a `page` when the document is long; table detection across a hundred pages is work you do not need.

On failure the text starts with `Table extraction failed for`, followed by the exception type and message.

## Example prompts

* *"Extract the tables from invoice.pdf."*
* *"Get the line-items table as JSON so you can total it."*
* *"Show me the table on page 4."*
* *"Pull the price table out of each of these PDFs."*

See it used end-to-end: [Extract tables into structured data]({{< ref "parser/mcp/use-cases/extract-tables-into-structured-data.md" >}}).
