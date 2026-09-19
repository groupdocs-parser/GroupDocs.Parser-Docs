---
id: mcp-uc-extract-document-data-with-ai-agents
url: parser/mcp/use-cases/extract-document-data-with-ai-agents
title: How to extract document data with AI agents using MCP
linkTitle: Extract with AI agents
weight: 1
description: "Extract text, tables, metadata, images, and barcodes from documents with an AI agent over MCP — locally, across 50+ formats."
keywords: extract document data AI agent, MCP document extraction, Claude read PDF locally, document parsing agent
productName: GroupDocs.Parser MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to extract document data with AI agents using MCP"
        description: "Extract text, tables, metadata, images, and barcodes from documents with an AI agent over MCP — locally, across 50+ formats."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Parser MCP server with Docker or dnx and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "How many pages is this, and does it have extractable text?"
---

Extraction is the step that turns a folder of documents into something an agent can reason about. Five tools cover the ways data hides in a file.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server, which ships as a Docker image only. Installation and client setup: [MCP server for .NET]({{< ref "parser/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## Which tool for which question

| You ask | Tool |
|---|---|
| "What does it say?" | [`extract_text`]({{< ref "parser/mcp/tools-reference/extract-text.md" >}}) |
| "What is in the table?" | [`extract_tables`]({{< ref "parser/mcp/tools-reference/extract-tables.md" >}}) |
| "Who wrote it, and when?" | [`extract_metadata`]({{< ref "parser/mcp/tools-reference/extract-metadata.md" >}}) |
| "Save the figures" | [`extract_images`]({{< ref "parser/mcp/tools-reference/extract-images.md" >}}) |
| "What does the barcode encode?" | [`extract_barcodes`]({{< ref "parser/mcp/tools-reference/extract-barcodes.md" >}}) |
| "How long is it, and what format?" | [`get_document_info`]({{< ref "parser/mcp/tools-reference/get-document-info.md" >}}) |

## The pattern

1. Put the documents in the storage folder the container mounts.
2. Ask: *"What does invoice.pdf say, and what is in its table?"*
3. The agent calls `extract_text` and `extract_tables` and reports both.
4. Everything happens in a local container — no upload.

## The rule that explains most surprises

`extract_text` reads a **text layer**. A born-digital PDF has one; a scan does not. On a scanned page the extraction returns little or nothing — which is a correct statement about the file, not a failure.

Check first when you are not sure:

> How many pages is this, and does it have extractable text?

[`get_document_info`]({{< ref "parser/mcp/tools-reference/get-document-info.md" >}}) plus a one-page text extraction answers it in two cheap calls. And on scans, [`extract_barcodes`]({{< ref "parser/mcp/tools-reference/extract-barcodes.md" >}}) still works — see [Read barcodes from scans]({{< ref "parser/mcp/use-cases/read-barcodes-from-scans.md" >}}).

## Long documents: go page by page

`extract_text` truncates very large outputs and marks where it stopped. For a complete read of a long document, extract per page:

> Extract the text of this 40-page report page by page and summarize each section.

## Setup

```bash
docker run --rm -i -v $(pwd)/documents:/data \
  ghcr.io/groupdocs-parser/parser-net-mcp:latest
```

**Docker only** — this product has no `dnx` channel ([why]({{< ref "parser/net/mcp/_index.md" >}})). Per-client config: [Register in AI clients]({{< ref "parser/net/mcp/install-in-ai-clients.md" >}}), or run the [installer]({{< ref "parser/mcp/getting-started/_index.md" >}}).

## Where to go next

* [Extract tables into structured data]({{< ref "parser/mcp/use-cases/extract-tables-into-structured-data.md" >}}) — Markdown to read, JSON to compute.
* [Read barcodes from scanned documents]({{< ref "parser/mcp/use-cases/read-barcodes-from-scans.md" >}}) — where text extraction stops.
* [Extract from a folder in bulk]({{< ref "parser/mcp/use-cases/bulk-extract-a-folder.md" >}}) — one prompt, many files.
* [On-premise architecture]({{< ref "parser/mcp/use-cases/on-premise-document-extraction.md" >}}) — the corpus stays put.
