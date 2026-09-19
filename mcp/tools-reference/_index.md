---
id: mcp-tools-reference
url: parser/mcp/tools-reference
title: Tools reference
weight: 2
description: "Complete reference of every tool the GroupDocs.Parser MCP server exposes to AI agents, with parameters, example prompts, and results."
keywords: MCP tools list document extraction, extract_text MCP tool, extract_tables MCP, extract barcodes MCP, MCP tools reference
productName: GroupDocs.Parser MCP Server
generated: true
serverVersion: 26.9.0
toc: True
---

Complete reference of every tool the GroupDocs.Parser MCP server exposes to AI agents, with parameters, example prompts, and results. Captured from a live `tools/list` call against server version **26.9.0** (raw capture: `tools-list.generated.json` in this section's source).

| Tool | What it does |
|---|---|
| [`extract_text`]({{< ref "parser/mcp/tools-reference/extract-text.md" >}}) | Extracts plain text from a document, whole or a single page |
| [`extract_tables`]({{< ref "parser/mcp/tools-reference/extract-tables.md" >}}) | Extracts tables as Markdown (renders in chat) or structured JSON |
| [`extract_metadata`]({{< ref "parser/mcp/tools-reference/extract-metadata.md" >}}) | Extracts author, title, dates, custom properties, EXIF, XMP, IPTC as JSON |
| [`extract_images`]({{< ref "parser/mcp/tools-reference/extract-images.md" >}}) | Extracts embedded images and saves them to storage as files |
| [`extract_barcodes`]({{< ref "parser/mcp/tools-reference/extract-barcodes.md" >}}) | Decodes barcodes and QR codes, returning values, types, and positions |
| [`get_document_info`]({{< ref "parser/mcp/tools-reference/get-document-info.md" >}}) | Returns file type, page count, and size without extracting anything |
| [`get_license_status`]({{< ref "parser/mcp/tools-reference/get-license-status.md" >}}) | Reports the active licensing mode and, under metered licensing, consumption |

## The FileInput shape

Every tool takes its document through the same `file` object — pass **either** a name from your storage folder **or** inline content:

```json
{ "file": { "filePath": "invoice.pdf" } }
```

| Field | Type | Description |
|---|---|---|
| `filePath` | string | File path or name in the configured storage folder |
| `fileContent` | string | Base64-encoded file content (alternative to `filePath`) |
| `fileName` | string | Original filename with extension — required with `fileContent`. Since **26.9.0** it also works on its own, resolved from the storage folder exactly like `filePath` |

You rarely write this JSON yourself: the AI agent does, from your plain-language prompt. Missing files are not an error to fear — the tool responds with the list of available files so the agent can correct itself.
