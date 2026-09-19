---
id: mcp-linux-installation
url: parser/net/mcp/linux-installation
title: Install on Linux
weight: 3
description: "Install the GroupDocs.Parser MCP server on Linux with the Docker engine and the guided installer — no .NET SDK and no native dependencies to install."
keywords: install MCP server Linux, MCP server docker Ubuntu, self-hosted document parsing Linux
productName: GroupDocs.Parser MCP Server for .NET
toc: True
---

Install the GroupDocs.Parser MCP server on **Linux** with the Docker engine and the guided installer:

```bash
git clone https://github.com/groupdocs/GroupDocs.Mcp.Installer.git
cd GroupDocs.Mcp.Installer
bash setup/linux.sh --channel docker
pwsh ./install-groupdocs-mcp.ps1 -Channel docker \
  -Products parser -Clients claude-desktop -Verify
```

{{< alert style="info" >}}
**Docker is the only channel for this product.** GroupDocs.Parser's engine embeds around 234 MB of ONNX models, which puts the packed tool over nuget.org's 250 MB package limit — so `dnx` and `dotnet tool install` are not available for this product. When a slimmer engine ships, the NuGet channel returns. That also means there are no `libgdiplus`-style native dependencies to install — everything is inside the image.
{{< /alert >}}

## Prerequisites

`setup/linux.sh` automates Debian/Ubuntu, idempotently; add `--check` to report without installing.

* **PowerShell 7 (`pwsh`)** — runs the installer scripts.
* **Docker engine** (`sudo apt-get install -y docker.io`); the script adds you to the `docker` group — **log out/in** (or `newgrp docker`) before the first run.

## Manual run

```bash
docker run --rm -i -v $(pwd)/documents:/data \
  ghcr.io/groupdocs-parser/parser-net-mcp:latest
```

Client config locations on Linux: Claude Desktop `~/.config/Claude/claude_desktop_config.json`, VS Code user-level `~/.config/Code/User/mcp.json` — full list in [Register in AI clients]({{< ref "parser/net/mcp/install-in-ai-clients.md" >}}).

## Verify

```bash
pwsh ./verify-groupdocs-mcp.ps1
```

A passing run completes the MCP handshake (seven tools listed) and — when a document sits in your storage folder — a real `get_document_info` call. For headless servers, `-EmitCompose` generates a `docker-compose.yml` for supervised operation.

## Linux-specific troubleshooting

* **`docker: permission denied`** — your user is not in the `docker` group yet; `newgrp docker` or re-login.
* **Empty `/data` in the container** — the volume path did not exist on the host, so Docker created an empty directory. Create the folder first, then run.
* **SELinux hosts** — add `:z` to the volume (`-v $(pwd)/documents:/data:z`) if the container cannot read mounted files.
* **Slow first start** — the image is large (it carries the ONNX models); pull it once with `docker pull ghcr.io/groupdocs-parser/parser-net-mcp:latest` before the first tool call.

Installing on a different OS? [Windows]({{< ref "parser/net/mcp/windows-installation.md" >}}) · [macOS]({{< ref "parser/net/mcp/macos-installation.md" >}})
