---
id: mcp-macos-installation
url: parser/net/mcp/macos-installation
title: Install on macOS
weight: 4
description: "Install the GroupDocs.Parser MCP server on macOS (Intel and Apple Silicon) with Docker Desktop and the guided installer — multi-arch images run natively on M-series Macs."
keywords: install MCP server macOS, MCP server Mac Apple Silicon docker, Claude Desktop MCP config Mac location
productName: GroupDocs.Parser MCP Server for .NET
toc: True
---

Install the GroupDocs.Parser MCP server on **macOS** — Intel and Apple Silicon — with Docker Desktop and the guided installer:

```bash
git clone https://github.com/groupdocs/GroupDocs.Mcp.Installer.git
cd GroupDocs.Mcp.Installer
bash setup/macos.sh --channel docker
pwsh ./install-groupdocs-mcp.ps1 -Channel docker \
  -Products parser -Clients claude-desktop,cursor -Verify
```

{{< alert style="info" >}}
**Docker is the only channel for this product.** GroupDocs.Parser's engine embeds around 234 MB of ONNX models, which puts the packed tool over nuget.org's 250 MB package limit — so `dnx` and `dotnet tool install` are not available for this product. When a slimmer engine ships, the NuGet channel returns.
{{< /alert >}}

## Prerequisites

`setup/macos.sh` installs via Homebrew, idempotently (add `--check` to report only):

* **PowerShell 7** — `brew install --cask powershell`.
* **Docker Desktop** — `brew install --cask docker`. Launch it once from /Applications to finish first-run setup.

**Apple Silicon:** the images are multi-arch (`linux/arm64` + `linux/amd64`), so M-series Macs run them **natively** — no Rosetta emulation.

## Manual run

```bash
docker run --rm -i -v $(pwd)/documents:/data \
  ghcr.io/groupdocs-parser/parser-net-mcp:latest
```

Client config locations on macOS: Claude Desktop `~/Library/Application Support/Claude/claude_desktop_config.json`, VS Code user-level `~/Library/Application Support/Code/User/mcp.json`, Cursor `~/.cursor/mcp.json` — full list in [Register in AI clients]({{< ref "parser/net/mcp/install-in-ai-clients.md" >}}).

## Verify

```bash
pwsh ./verify-groupdocs-mcp.ps1
```

The handshake lists the server's seven tools; with a document in your storage folder it also runs a real `get_document_info` call through the engine.

## macOS-specific troubleshooting

* **Docker daemon not reachable** — Docker Desktop must be running (menu-bar whale icon).
* **File sharing** — Docker Desktop only mounts paths it is allowed to share (Settings → Resources → File sharing); a folder outside those paths mounts empty.
* **`pwsh` not found after install** — open a new terminal so the shell picks up the Homebrew path.
* **First pull is large** — the image carries the ONNX models; `docker pull ghcr.io/groupdocs-parser/parser-net-mcp:latest` once, before your client first needs it.

Installing on a different OS? [Windows]({{< ref "parser/net/mcp/windows-installation.md" >}}) · [Linux]({{< ref "parser/net/mcp/linux-installation.md" >}})
