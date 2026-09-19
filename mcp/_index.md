---
id: mcp
url: parser/mcp
title: GroupDocs.Parser MCP Server
weight: 6
description: "GroupDocs.Parser MCP server lets AI agents like Claude, Cursor, and Copilot extract text, tables, images, metadata, and barcodes from documents — locally on your machine."
keywords: document extraction MCP server, extract text from PDF AI agent, extract tables MCP, barcode extraction MCP, Claude read documents locally
productName: GroupDocs.Parser MCP Server
hideChildren: True
toc: True
---

**GroupDocs.Parser MCP server** lets AI agents like Claude, Cursor, and Copilot **pull data out of documents** — text, tables, images, metadata, barcodes — from PDF, Office, email, e-book, and image formats, **locally on your machine**. Install with one command:

```bash
docker run --rm -i -v $(pwd)/documents:/data \
  ghcr.io/groupdocs-parser/parser-net-mcp:latest
```

{{< alert style="info" >}}
**This product ships via Docker only.** GroupDocs.Parser's engine embeds around 234 MB of ONNX models, which puts the packed tool over nuget.org's 250 MB package limit — so this product ships **via Docker only**, and there is no `dnx` command for it. Other GroupDocs MCP servers offer a NuGet (`dnx`) channel; this one does not, and every command in these pages reflects that. Details: [MCP server for .NET]({{< ref "parser/net/mcp/_index.md" >}}).
{{< /alert >}}

Or use the [guided installer]({{< ref "parser/mcp/getting-started/_index.md" >}}) to register the server in your AI client, verify the setup, and configure shared folders in one pass.

## What you can do

Seven tools (full details in the [tools reference]({{< ref "parser/mcp/tools-reference/_index.md" >}})):

* **[`extract_text`]({{< ref "parser/mcp/tools-reference/extract-text.md" >}})** — plain text, whole document or one page.
* **[`extract_tables`]({{< ref "parser/mcp/tools-reference/extract-tables.md" >}})** — tables as Markdown that renders in chat, or JSON rows to compute with.
* **[`extract_metadata`]({{< ref "parser/mcp/tools-reference/extract-metadata.md" >}})** — author, title, dates, custom properties, EXIF/XMP/IPTC.
* **[`extract_images`]({{< ref "parser/mcp/tools-reference/extract-images.md" >}})** — embedded images saved to storage as files.
* **[`extract_barcodes`]({{< ref "parser/mcp/tools-reference/extract-barcodes.md" >}})** — Code128, QR, PDF417, DataMatrix, EAN, UPC, Aztec and more — **including on scans**.
* **[`get_document_info`]({{< ref "parser/mcp/tools-reference/get-document-info.md" >}})** — type, pages, size, without extracting.
* **[`get_license_status`]({{< ref "parser/mcp/tools-reference/get-license-status.md" >}})** — active licensing mode and metered consumption.

Ask in plain language — *"what does this invoice say, and what is in its table?"* — and the agent picks the tools.

## Install for your platform

Installation, prerequisites, and client configuration are platform-specific; the tools and licensing model below are the same everywhere.

| Platform | Status | Install and setup |
|---|---|---|
| .NET | **Available** (Docker only) | [MCP server for .NET]({{< ref "parser/net/mcp/_index.md" >}}) |
| Java | Planned | [Tell us you need it](https://forum.groupdocs.com/c/parser/17) |
| Python | Planned | [Tell us you need it](https://forum.groupdocs.com/c/parser/17) |
| Node.js | Planned | [Tell us you need it](https://forum.groupdocs.com/c/parser/17) |

## Extraction is not conversion

The distinction that decides which GroupDocs server you want:

| You want | Server |
|---|---|
| Values **out of** a document — text, a table, a barcode, properties | **this one** |
| The document **as another format**, layout preserved | [GroupDocs.Conversion](/conversion/mcp/) |
| Clean **Markdown** of the whole document for RAG | [GroupDocs.Markdown](/markdown/mcp/) |
| Metadata **edited or removed**, not just read | [GroupDocs.Metadata](/metadata/mcp/) |

Plenty of pipelines use two: convert for ingestion, parse for extraction.

## Barcodes work where text does not

A scanned page has no text layer, so [`extract_text`]({{< ref "parser/mcp/tools-reference/extract-text.md" >}}) returns little — but the engine's models detect **codes in the rasterized image**, so [`extract_barcodes`]({{< ref "parser/mcp/tools-reference/extract-barcodes.md" >}}) still returns the tracking number on a scanned delivery note. That capability is also why the image is large: the models ship inside it.

There is **no OCR**: printed words in a scan stay pixels.

## Supported AI clients

| Client | How it connects |
|---|---|
| Claude Desktop | `claude_desktop_config.json` |
| Claude Code | `claude mcp add` CLI |
| VS Code / GitHub Copilot | user-level or workspace `mcp.json` |
| Visual Studio 2022 (17.14+) | `.mcp.json` in the solution root |
| Cursor | `~/.cursor/mcp.json` |
| Windsurf | `~/.codeium/windsurf/mcp_config.json` |
| Cline | Cline MCP settings |
| Codex CLI | `codex mcp add` CLI |
| JetBrains Rider | manual registration (Settings → AI Assistant → MCP) |

Exact config blocks for every client: [Register in AI clients]({{< ref "parser/net/mcp/install-in-ai-clients.md" >}}).

## Delivery channel

| | Docker (the only channel) |
|---|---|
| Prerequisites | Docker. No .NET SDK — the runtime is in the image |
| Native dependencies | bundled in the image |
| Image | `ghcr.io/groupdocs-parser/parser-net-mcp` · `groupdocs/parser-net-mcp` |
| Architectures | linux/amd64 + linux/arm64 (Apple Silicon native) |
| Note | the image is large (it carries the ONNX models) — pull it once, deliberately |

## How it works

The server uses MCP's **local stdio transport**: your AI client starts the container as a child process and talks to it over standard input/output. No inbound ports, no external endpoints, no telemetry — the data path is *agent → local server → local filesystem*. Extraction is where document content most obviously leaves a building; here it does not. Details: [On-premise architecture]({{< ref "parser/mcp/use-cases/on-premise-document-extraction.md" >}}).

## When you need more than a text-extraction script

Scripts that pull text from PDFs are easy to find and hard to trust across formats. Choose this server when you need: **one interface across 50+ formats** including email and e-books; **tables as tables**, not as a soup of positioned strings; barcode decoding that works on scans; metadata across documents and images in the same call; and the fidelity of the commercial GroupDocs engine trusted by enterprise teams for over a decade.

## Resources

* [Quick start]({{< ref "parser/mcp/getting-started/_index.md" >}}) · [Use cases]({{< ref "parser/mcp/use-cases/_index.md" >}}) · [Troubleshooting & FAQ]({{< ref "parser/mcp/troubleshooting-faq.md" >}})
* GitHub: [server source](https://github.com/groupdocs-parser/GroupDocs.Parser.Mcp) · [installer](https://github.com/groupdocs/GroupDocs.Mcp.Installer) · [integration tests](https://github.com/groupdocs-parser/GroupDocs.Parser.Mcp.Tests)
* [Docker image](https://github.com/orgs/groupdocs-parser/packages/container/package/parser-net-mcp) · [MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=io.github.groupdocs-parser/groupdocs-parser-mcp)
* Questions: [Parser forum](https://forum.groupdocs.com/c/parser/17)
