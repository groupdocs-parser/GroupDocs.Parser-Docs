---
id: mcp-net-troubleshooting
url: parser/net/mcp/troubleshooting
title: Troubleshooting (.NET)
linkTitle: Troubleshooting
weight: 9
description: "Fixes for problems specific to the Docker build of the GroupDocs.Parser MCP server — daemon not reachable, empty volumes, slow first pull, and version pinning."
keywords: docker daemon not reachable, MCP server empty data folder, docker volume not mounted MCP, MCP server first launch timeout
productName: GroupDocs.Parser MCP Server for .NET
toc: True
---

Problems specific to running this server — which means **Docker problems**, since Docker is its only channel. For issues that are the same on every platform (the server not appearing in your client, evaluation limits, engine behaviour), see [Troubleshooting & FAQ]({{< ref "parser/mcp/troubleshooting-faq.md" >}}).

## Why is there no `dnx` command for this product?

GroupDocs.Parser's engine embeds around 234 MB of ONNX models, which puts the packed tool over nuget.org's 250 MB package limit — so `dnx` and `dotnet tool install` are not available for this product. When a slimmer engine ships, the NuGet channel returns. If you have seen `dnx GroupDocs.<Product>.Mcp --yes` in another GroupDocs MCP guide, that is correct for those products and does not apply here.

## "docker daemon not reachable"

Start Docker Desktop (or `dockerd`). The installer's preflight detects this **before** writing any config and prints the exact `setup/<os>` command that fixes the environment.

## The agent says the storage folder is empty

The volume did not mount what you expected. Three usual causes:

* **The host path does not exist** — Docker silently creates an empty directory instead of failing.
* **File sharing is not enabled** for that location (Docker Desktop → Settings → Resources → File sharing) on Windows and macOS.
* **Windows path form** — use forward slashes: `-v C:/Docs:/data`.

Check by listing from inside the container:

```bash
docker run --rm -v /path/to/documents:/data \
  ghcr.io/groupdocs-parser/parser-net-mcp:latest ls /data
```

## Paths in the config are container paths

`GROUPDOCS_MCP_STORAGE_PATH=/data` refers to the path **inside** the container — `/data` is whatever host folder you mounted there. A host path such as `D:/Storage/Documents` in that variable means nothing to the container and produces an empty storage folder. See [Configuration]({{< ref "parser/net/mcp/configuration.md" >}}).

## The first tool call is slow, or times out once and then works

The image is **large** — it carries the engine's ONNX models — and on first use your client is waiting while Docker pulls it. Warm it once:

```bash
docker pull ghcr.io/groupdocs-parser/parser-net-mcp:latest
```

The installer's `-Verify` mode pre-warms automatically, and `./install-groupdocs-mcp.ps1 -Prewarm` does it on demand.

## Pinning a version

`:latest` resolves to the newest release whenever the container starts. For shared or committed configurations, pin the tag — `ghcr.io/groupdocs-parser/parser-net-mcp:26.9.0` — so a release cannot change behaviour under a running workflow.

## Verifying an installation end-to-end

```powershell
./verify-groupdocs-mcp.ps1
```

Performs the real MCP handshake per configured product and — when a document exists in your storage folder — a live `get_document_info` call through the engine. Exit `0` = healthy; failures print the server's own error text.

## Still stuck?

Post your config (redact license paths) and the client name in the [Parser forum](https://forum.groupdocs.com/c/parser/17). Bugs: [GitHub issues](https://github.com/groupdocs-parser/GroupDocs.Parser.Mcp/issues).
