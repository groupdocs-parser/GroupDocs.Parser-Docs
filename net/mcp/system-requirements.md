---
id: mcp-system-requirements
url: parser/net/mcp/system-requirements
title: System requirements
weight: 8
description: "The GroupDocs.Parser MCP server runs anywhere Docker runs — this page lists the exact requirements, image size considerations, and supported architectures."
keywords: MCP server requirements, does MCP server need Docker, parser MCP docker requirements
productName: GroupDocs.Parser MCP Server for .NET
toc: True
---

The GroupDocs.Parser MCP server runs **anywhere Docker runs**. There is no NuGet channel for this product — GroupDocs.Parser's engine embeds around 234 MB of ONNX models, which puts the packed tool over nuget.org's 250 MB package limit — so `dnx` and `dotnet tool install` are not available for this product. When a slimmer engine ships, the NuGet channel returns.

| | Docker channel (the only channel) |
|---|---|
| Runtime | Docker Desktop / docker engine |
| .NET SDK | **not required** — the runtime is inside the image |
| Native dependencies | bundled in the image |
| Architectures | linux/amd64 + linux/arm64 (Apple Silicon native) |
| Installer scripts | PowerShell 5.1 (Windows built-in) or PowerShell 7 (`pwsh`, macOS/Linux) |
| Disk | the image is large — it carries the engine's ONNX models. Pull once per version |
| Network | pull from ghcr.io / docker.io at install only — no network at runtime |

**Plan for the image size.** The models that make text and barcode extraction work are inside the image, so the first pull is substantially bigger than for the other GroupDocs MCP servers. Pull it deliberately (`docker pull ghcr.io/groupdocs-parser/parser-net-mcp:latest`) rather than letting a client hit it on the first tool call and time out.

Per-OS setup, including the one-shot prerequisite bootstrappers (`setup/windows.ps1`, `setup/linux.sh`, `setup/macos.sh`):

* [Install on Windows]({{< ref "parser/net/mcp/windows-installation.md" >}})
* [Install on Linux]({{< ref "parser/net/mcp/linux-installation.md" >}})
* [Install on macOS]({{< ref "parser/net/mcp/macos-installation.md" >}})

AI client requirements: any MCP-compatible client (Claude Desktop, Claude Code, VS Code / GitHub Copilot, Visual Studio 2022 17.14+, Cursor, Windsurf, Cline, Codex CLI) — see [Register in AI clients]({{< ref "parser/net/mcp/install-in-ai-clients.md" >}}).

The engine's own platform requirements are those of the library: [GroupDocs.Parser system requirements]({{< ref "parser/net/getting-started/system-requirements.md" >}}).
