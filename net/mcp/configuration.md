---
id: mcp-configuration
url: parser/net/mcp/configuration
title: Configuration
weight: 6
description: "Configure the GroupDocs.Parser MCP server: volume mounts, storage and output folders, license path, and metered keys — all through the Docker run command or the installer config file."
keywords: MCP server environment variables, docker volume MCP server, MCP server storage path, pin MCP server version
productName: GroupDocs.Parser MCP Server for .NET
toc: True
---

Configure the GroupDocs.Parser MCP server through **one config file** — the installer's `groupdocs-mcp.config.json` — or directly on the `docker run` command line:

```json
{
  "channel":     "docker",
  "registry":    "ghcr",
  "clients":     ["claude-desktop", "vscode"],
  "version":     "latest",
  "storagePath": "D:/Storage/Documents",
  "outputPath":  "D:/Storage/Output",
  "licensePath": "D:/Storage/Licenses/GroupDocs.Parser.lic",
  "products":    ["parser"]
}
```

* `channel` — `docker` is the only option for this product.
* `storagePath` — the host folder mounted into the container as `/data` (created if missing).
* `outputPath` — optional separate folder for extracted images and files; empty = same as storage.
* `licensePath` — empty string = evaluation mode (safe, no error); see [Licensing]({{< ref "parser/mcp/getting-started/licensing.md" >}}).
* `version` — `"latest"` or a pin such as `"26.9.0"`. Pinning is recommended for shared/committed configs.

## Environment variables

The same settings travel as environment variables, passed **before the image name**:

| Variable | Description | Default |
|---|---|---|
| `GROUPDOCS_MCP_STORAGE_PATH` | Base folder **inside the container** for input and output — normally `/data` | current directory |
| `GROUPDOCS_MCP_OUTPUT_PATH` | Optional separate output folder inside the container | same as storage |
| `GROUPDOCS_LICENSE_PATH` | Path **inside the container** to a GroupDocs license file | empty = evaluation mode |
| `GROUPDOCS_METERED_PUBLIC_KEY` | Metered public key. Requires the private key too; takes precedence over the license file | unset |
| `GROUPDOCS_METERED_PRIVATE_KEY` | Metered private key. **Treat as a secret** — see [Licensing]({{< ref "parser/mcp/getting-started/licensing.md" >}}) | unset |

The distinction that trips people up: these paths are **container paths**, not host paths. `/data` is whatever host folder you mounted there.

## Volume mapping

```bash
docker run --rm -i \
  -v /path/to/documents:/data \
  -v /path/to/license-folder:/license:ro \
  -e GROUPDOCS_MCP_STORAGE_PATH=/data \
  -e GROUPDOCS_LICENSE_PATH=/license/GroupDocs.Parser.lic \
  ghcr.io/groupdocs-parser/parser-net-mcp:latest
```

* `storagePath` → `/data`
* `outputPath` → `/data/output` (when set)
* license folder → `/license:ro`, read-only

Metered keys can be forwarded **by name** so they never appear in a config file:

```bash
docker run --rm -i -v /path/to/documents:/data \
  -e GROUPDOCS_MCP_STORAGE_PATH=/data \
  -e GROUPDOCS_METERED_PUBLIC_KEY -e GROUPDOCS_METERED_PRIVATE_KEY \
  ghcr.io/groupdocs-parser/parser-net-mcp:latest
```

## Compose-only setups

For container fleets without client registration, generate a `docker-compose.yml`:

```powershell
./install-groupdocs-mcp.ps1 -EmitCompose -Clients @()
```
