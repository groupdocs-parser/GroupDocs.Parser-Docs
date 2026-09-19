---
id: mcp-net
url: parser/net/mcp
title: MCP server for .NET
linkTitle: MCP Server
weight: 7
description: "Install and configure the GroupDocs.Parser MCP server for .NET — a Docker-only product: one-click install links, per-OS Docker setup, and the full environment-variable reference."
keywords: GroupDocs.Parser MCP .NET, MCP server Docker image, parser MCP install, Model Context Protocol .NET
productName: GroupDocs.Parser MCP Server for .NET
hideChildren: True
toc: True
---

Everything needed to **install and run** the GroupDocs.Parser MCP server on the .NET platform. What the server *does* — its tools, use cases, and licensing model — is platform-independent and lives in the [MCP server section]({{< ref "parser/mcp/_index.md" >}}).

{{< alert style="info" >}}
**This product ships via Docker only.** GroupDocs.Parser's engine embeds around 234 MB of ONNX models, which puts the packed tool over nuget.org's 250 MB package limit — so `dnx` and `dotnet tool install` are not available for this product. When a slimmer engine ships, the NuGet channel returns.
{{< /alert >}}

| The .NET build at a glance | |
|---|---|
| Container images | `ghcr.io/groupdocs-parser/parser-net-mcp` · `groupdocs/parser-net-mcp` |
| One-command run | `docker run --rm -i -v $(pwd)/documents:/data ghcr.io/groupdocs-parser/parser-net-mcp:latest` |
| Current version | **26.9.0** (tagged `:latest` and `:26.9.0`) |
| Prerequisites | Docker. No .NET SDK needed — the runtime is inside the image |
| Architectures | linux/amd64 + linux/arm64 (Apple Silicon native) |
| Source | [GroupDocs.Parser.Mcp on GitHub](https://github.com/groupdocs-parser/GroupDocs.Parser.Mcp) |
| Release notes | [changelog](https://github.com/groupdocs-parser/GroupDocs.Parser.Mcp/tree/master/changelog) · [GitHub releases](https://github.com/groupdocs-parser/GroupDocs.Parser.Mcp/releases) |

## Start here

1. **Install Docker** for your operating system — [Windows]({{< ref "parser/net/mcp/windows-installation.md" >}}) · [Linux]({{< ref "parser/net/mcp/linux-installation.md" >}}) · [macOS]({{< ref "parser/net/mcp/macos-installation.md" >}})
2. **Register it in your AI client** — [one-click links and per-client configs]({{< ref "parser/net/mcp/install-in-ai-clients.md" >}})
3. **Point it at your documents** — [configuration]({{< ref "parser/net/mcp/configuration.md" >}})
4. **License it** — evaluation, a license file, or metered keys: [Licensing]({{< ref "parser/mcp/getting-started/licensing.md" >}})

## In this section

* [Install on Windows]({{< ref "parser/net/mcp/windows-installation.md" >}})
* [Install on Linux]({{< ref "parser/net/mcp/linux-installation.md" >}})
* [Install on macOS]({{< ref "parser/net/mcp/macos-installation.md" >}})
* [Register in AI clients]({{< ref "parser/net/mcp/install-in-ai-clients.md" >}}) — VS Code, Cursor, Claude, Visual Studio, Windsurf, Cline, Codex, Rider
* [Configuration]({{< ref "parser/net/mcp/configuration.md" >}}) — volumes, storage, output, license, metered keys
* [System requirements]({{< ref "parser/net/mcp/system-requirements.md" >}})
* [Troubleshooting (.NET)]({{< ref "parser/net/mcp/troubleshooting.md" >}}) — Docker daemon, volumes, image pulls

## Platform-independent reference

* [Tools reference]({{< ref "parser/mcp/tools-reference/_index.md" >}}) — `extract_text`, `extract_tables`, `extract_metadata`, `extract_images`, `extract_barcodes`, `get_document_info`, `get_license_status`
* [Use cases]({{< ref "parser/mcp/use-cases/_index.md" >}}) · [Supported formats]({{< ref "parser/mcp/supported-formats.md" >}}) · [Troubleshooting & FAQ]({{< ref "parser/mcp/troubleshooting-faq.md" >}})
