---
id: mcp-troubleshooting-faq
url: parser/mcp/troubleshooting-faq
title: Troubleshooting & FAQ
weight: 5
description: "Solutions to the most common GroupDocs.Parser MCP server issues — server not appearing in the client, startup failures, missing native dependencies, and first-launch timeouts."
keywords: MCP server not showing up in Claude Desktop, Claude can't see MCP tools, MCP server failed to start, docker daemon not reachable, docker volume not mounted, extract text from PDF AI, extract tables from document MCP, read barcodes PDF agent
productName: GroupDocs.Parser MCP Server
toc: True
---

Solutions to the most common GroupDocs.Parser MCP server issues — server not appearing in the client, startup failures, missing native dependencies, and first-launch timeouts.

{{< alert style="info" >}}
**Platform-specific troubleshooting:** this product runs as a Docker container, so runtime problems are Docker problems — the daemon, volume mounts, and the first image pull. See [Troubleshooting (.NET)]({{< ref "parser/net/mcp/troubleshooting.md" >}}). The issues on this page apply to every platform.
{{< /alert >}}

## Why is my MCP server not showing up in Claude Desktop?

1. **Restart the client** — every client reads its MCP config only at startup.
2. Check the config file location for your OS ([per-client reference]({{< ref "parser/net/mcp/install-in-ai-clients.md" >}})) and that the entry sits under the right root key (`mcpServers` for Claude Desktop/Cursor/Windsurf, `servers` for VS Code/VS 2022).
3. Validate the JSON — a trailing comma silently breaks the whole file. If you used the [installer](https://github.com/groupdocs/GroupDocs.Mcp.Installer), a timestamped `.bak` of your previous config sits next to the file for comparison.

## The first tool call is slow or fails once, then works

A **cold cache**: on the very first use the server's package or image is still downloading while the client is already waiting on the connection. Warming it once fixes it for good — the exact command depends on your build: [.NET]({{< ref "parser/net/mcp/troubleshooting.md" >}}#the-first-tool-call-is-slow-or-times-out-once-and-then-works).

## The server fails to start, or a runtime dependency is missing

These are properties of the build you run rather than of MCP, so the fixes live with the platform:

| Symptom | Where the fix is |
|---|---|
| "docker daemon not reachable" | [.NET troubleshooting]({{< ref "parser/net/mcp/troubleshooting.md" >}}#docker-daemon-not-reachable) — start Docker Desktop or `dockerd` |
| The storage folder looks empty to the agent | [.NET troubleshooting]({{< ref "parser/net/mcp/troubleshooting.md" >}}#the-agent-says-the-storage-folder-is-empty) — the volume did not mount what you expected |
| First call times out, later calls work | [.NET troubleshooting]({{< ref "parser/net/mcp/troubleshooting.md" >}}#the-first-tool-call-is-slow-or-times-out-once-and-then-works) — the image is large; pull it once |
| You were told to run `dnx` | There is no NuGet package for this product — [why]({{< ref "parser/net/mcp/troubleshooting.md" >}}#why-is-there-no-dnx-command-for-this-product) |

## The agent says a file does not exist

Pass the **file name**, not a full path from your machine: the server resolves names inside its configured storage folder. When a name is not found the tool responds with the list of files it can see, so the agent can correct itself — check that list against [`GROUPDOCS_MCP_STORAGE_PATH`]({{< ref "parser/net/mcp/configuration.md" >}}).

## Why is there no `dnx` command for Parser?

Because there is no NuGet package. GroupDocs.Parser's engine embeds around **234 MB of ONNX models**, which puts the packed tool over nuget.org's 250 MB limit — so this product ships **via Docker only**. Every install page here uses `docker run`; when a slimmer engine ships, the NuGet channel returns. See [Install for .NET]({{< ref "parser/net/mcp/_index.md" >}}).

## Extraction or conversion — which do I need?

**Extraction** pulls values *out* of a document: text, tables, barcodes, metadata. **Conversion** turns a document into a different format, preserving layout. If you want a DOCX as a PDF, that is the [GroupDocs.Conversion MCP server](/conversion/mcp/); if you want the numbers out of the table on page 3, it is this one. Many pipelines use both.

## Does it OCR scanned documents?

Text extraction works on the text layer. The engine's models do power **barcode and QR detection on rasterized content**, so [`extract_barcodes`]({{< ref "parser/mcp/tools-reference/extract-barcodes.md" >}}) works on scans where [`extract_text`]({{< ref "parser/mcp/tools-reference/extract-text.md" >}}) returns nothing. An empty text extraction on a scanned page means "no text layer", not "empty page".

## Why was my text truncated?

[`extract_text`]({{< ref "parser/mcp/tools-reference/extract-text.md" >}}) truncates very large outputs and marks where it stopped, so a 900-page book does not arrive as one response. Extract **per page** (`page: 12`) when you need completeness, and loop.

## Markdown or JSON for tables?

`format: "markdown"` (the default) renders as a real table in chat, VS Code, and GitHub — best for looking at data. `format: "json"` returns rows as structured data — best when the agent will compute with it or write it somewhere. Ask for the one that matches what happens next.

## Verifying an installation end-to-end

Ask your agent *"list your GroupDocs parser tools and the license status"* — it should name `extract_text`, `extract_tables`, `extract_metadata`, `extract_images`, `extract_barcodes`, `get_document_info`, `get_license_status`. For a scripted check that performs the real MCP handshake and a live call through the engine, see [verifying a .NET installation]({{< ref "parser/net/mcp/troubleshooting.md" >}}#verifying-an-installation-end-to-end).

## Still stuck?

Post your config (redact license paths) and the client name in the [Parser forum](https://forum.groupdocs.com/c/parser/17) — we answer MCP questions daily. Bugs: [GitHub issues](https://github.com/groupdocs-parser/GroupDocs.Parser.Mcp/issues).
