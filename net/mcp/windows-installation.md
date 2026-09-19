---
id: mcp-windows-installation
url: parser/net/mcp/windows-installation
title: Install on Windows
weight: 2
description: "Install the GroupDocs.Parser MCP server on Windows with Docker Desktop and the guided installer — it registers the server in Claude Desktop, VS Code, and other clients automatically."
keywords: install MCP server Windows, MCP server Docker Desktop Windows, Claude Desktop MCP Windows config location
productName: GroupDocs.Parser MCP Server for .NET
toc: True
---

Install the GroupDocs.Parser MCP server on **Windows** with Docker Desktop and the guided installer:

```powershell
git clone https://github.com/groupdocs/GroupDocs.Mcp.Installer.git
cd GroupDocs.Mcp.Installer
powershell -ExecutionPolicy Bypass -File setup\windows.ps1 -Channel docker
./install-groupdocs-mcp.ps1 -Channel docker -Products parser `
  -Clients claude-desktop,vscode -Verify
```

The built-in Windows PowerShell 5.1 is enough — PowerShell 7 is optional.

{{< alert style="info" >}}
**Docker is the only channel for this product.** GroupDocs.Parser's engine embeds around 234 MB of ONNX models, which puts the packed tool over nuget.org's 250 MB package limit — so `dnx` and `dotnet tool install` are not available for this product. When a slimmer engine ships, the NuGet channel returns.
{{< /alert >}}

## Prerequisites

Docker Desktop, installed by the bootstrapper via winget (`winget install --id Docker.DockerDesktop`) or by you. **Start Docker Desktop once** after installing so it finishes its WSL2 first-run setup — the installer's preflight checks this and tells you if the daemon is unreachable.

No .NET SDK is required: the runtime and every native dependency live inside the image.

## Manual run

```powershell
docker run --rm -i -v C:/Docs:/data `
  ghcr.io/groupdocs-parser/parser-net-mcp:latest
```

Client config locations on Windows: Claude Desktop `%APPDATA%\Claude\claude_desktop_config.json`, VS Code user-level `%APPDATA%\Code\User\mcp.json` — full list with JSON blocks in [Register in AI clients]({{< ref "parser/net/mcp/install-in-ai-clients.md" >}}).

## Verify

```powershell
./verify-groupdocs-mcp.ps1
```

A passing run performs the MCP handshake and prints the server's seven tools; with a document in your storage folder it also runs a real `get_document_info` call. Exit code `0` means everything works.

## Windows-specific troubleshooting

* **"running scripts is disabled on this system"** — launch with `powershell -ExecutionPolicy Bypass -File …` (as shown above), or `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`.
* **"docker daemon not reachable"** — start Docker Desktop; the installer's preflight detects this before writing any config.
* **Drive sharing** — Docker Desktop must be allowed to mount the drive your documents live on (Settings → Resources → File sharing) or the container starts with an empty `/data`.
* **Path form** — use forward slashes in the volume argument (`C:/Docs:/data`); backslashes confuse the Docker CLI on Windows.

Installing on a different OS? [Linux]({{< ref "parser/net/mcp/linux-installation.md" >}}) · [macOS]({{< ref "parser/net/mcp/macos-installation.md" >}})
