---
id: sdk-cloud-mcp
url: parser/python-net/sdk-cloud-mcp
title: SDK, Cloud or MCP?
linkTitle: SDK, Cloud or MCP?
weight: 11
description: "Choose between GroupDocs.Parser for Python via .NET (the on-premise SDK), GroupDocs.Parser Cloud (a hosted REST API) and the GroupDocs.Parser MCP server (extraction tools for AI agents)."
keywords: GroupDocs.Parser, Python, SDK, Cloud, REST API, MCP, Model Context Protocol, AI agents, data extraction, decision guide
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
---

GroupDocs.Parser reaches Python code in three ways. They share the extraction engine but differ in where the documents are processed and who writes the code. This page helps you pick one.

## At a glance

| | GroupDocs.Parser for Python via .NET (SDK) | GroupDocs.Parser Cloud | GroupDocs.Parser MCP server |
|---|---|---|---|
| **What it is** | A Python package (`pip install groupdocs-parser-net`) | A hosted REST API with a Python SDK (`pip install groupdocs-parser-cloud`) | A Model Context Protocol server with extraction tools |
| **Who calls it** | Your Python code | Your code, over HTTPS | An AI agent (Claude, Cursor, GitHub Copilot and other MCP clients) |
| **Where documents go** | Stay on your machine or server | Uploaded to GroupDocs Cloud storage | Stay on the machine that runs the server |
| **Internet access** | Not needed | Required | Not needed once installed |
| **What you can do** | The whole API: text (plain, Markdown, HTML, OCR), search, tables, images, metadata, hyperlinks, barcodes, annotations, table of contents, templates and forms, archives and attachments, page previews | Parsing through REST endpoints; see its documentation for the operations it exposes | Seven tools: text, tables, metadata, images, barcodes, document info and license status |
| **Runs on** | Windows x64, Linux x64, macOS 13+ (Intel and Apple Silicon; OCR needs 13.3+); Python 3.5 - 3.14 | Any platform that can make HTTPS calls | A Docker image of the .NET server |
| **Licensing** | GroupDocs.Parser license (evaluation mode without one) | GroupDocs Cloud subscription | GroupDocs.Parser license (evaluation mode without one) |

## Choose the SDK when

- Documents must not leave your environment (compliance, air-gapped systems, on-premise deployments).
- You need the whole API: template-based parsing, search and highlights, text areas with coordinates, annotations, OCR, page previews, or the documents inside archives and emails.
- You process many documents in batch jobs and want no network round trips.

Start with [Installation]({{< ref "parser/python-net/getting-started/installation" >}}) and the [Quick Start Guide]({{< ref "parser/python-net/getting-started/quick-start" >}}). For shell scripts and CI steps, the package also installs the [`groupdocs-parser` command]({{< ref "parser/python-net/developer-guide/command-line-interface" >}}).

## Choose GroupDocs.Parser Cloud when

- You prefer a managed service to installing and updating a package.
- Your application already works with cloud storage, or runs where installing a 300 MB wheel or native packages is not possible.

See [GroupDocs.Parser Cloud for Python](https://products.groupdocs.cloud/parser/python/) and its [documentation](https://docs.groupdocs.cloud/parser/).

## Choose the MCP server when

- An AI agent should read documents itself, as a step in a conversation or a workflow, without you writing extraction code.
- You want local processing for an agent: the server runs next to your project, and documents stay on that machine.

See the [GroupDocs.Parser MCP server]({{< ref "parser/mcp/_index.md" >}}) documentation. The server is the .NET build of GroupDocs.Parser and ships as a Docker image, so it runs alongside a Python project without a .NET installation.

## Combine them

- **SDK + documentation MCP server**: when an AI coding assistant writes Python code against this package, connect it to the [documentation MCP server](https://docs.groupdocs.com/mcp) and let it read the `AGENTS.md` shipped inside the package. See [Agents and LLM Integration]({{< ref "parser/python-net/agents-and-llm-integration.md" >}}).
- **SDK in production, MCP server for prototyping**: let an agent try extraction scenarios through the MCP server, then implement the chosen workflow with the SDK.
