---
id: mcp-uc-on-premise-document-extraction
url: parser/mcp/use-cases/on-premise-document-extraction
title: "Running GroupDocs MCP servers on-premise: architecture and security model"
linkTitle: On-premise deployment
weight: 5
description: "Run document extraction for AI agents fully on-premise with the Docker-only Parser server: local stdio transport, no external endpoints, no telemetry."
keywords: on-premise MCP server, air-gapped document extraction, MCP security model, local document parsing no upload
productName: GroupDocs.Parser MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "Running GroupDocs MCP servers on-premise: architecture and security model"
        description: "Run document extraction for AI agents fully on-premise with the Docker-only Parser server: local stdio transport, no external endpoints, no telemetry."
        steps:
        - name: "Run the pinned image inside the perimeter"
          text: "Start the GroupDocs.Parser MCP server from its versioned Docker image as a child process of the AI client."
        - name: "Mount only the folders the agent may reach"
          text: "Map the document folder read-write and the license folder read-only."
        - name: "Choose the license mode"
          text: "Use a license file for fully offline operation; metered licensing needs outbound egress for usage reports."
---

Run document extraction for AI agents **fully on-premise**: the GroupDocs.Parser MCP server uses local stdio transport with **no external endpoints, no inbound ports, and no telemetry**. This page is the one to send your security reviewer.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server, which ships as a Docker image only. Installation and client setup: [MCP server for .NET]({{< ref "parser/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## The architecture in one picture

```text
+--------------+          +--------------------+         +------------------+
|  AI client   |  stdio   | MCP server process | reads / | local filesystem |
| (Claude, VS  | <----->  | (GroupDocs engine) | <-----> | storage / output |
| Code, agent) | JSON-RPC |   child process    |  writes |     folders      |
+--------------+          +--------------------+         +------------------+
```

Here the "MCP server process" is a **container**: your AI client runs `docker run --rm -i …` as a child process and talks to it over standard input/output. The transport properties are identical to the other GroupDocs MCP servers.

* **Transport:** stdio to a child process. The server never listens on a network socket.
* **Data path:** agent → local container → mounted folder. Documents are read from the volume you mounted; no content is transmitted anywhere.
* **Network use:** only at install time (pulling the image from ghcr.io/docker.io). At runtime the container makes no outbound calls. Air-gapped: pre-pull the image and pin the tag.
* **Telemetry:** none.

## Docker-only, and why that is a feature here

GroupDocs.Parser's engine embeds around 234 MB of ONNX models, which puts the packed tool over nuget.org's 250 MB package limit — so this product ships **via Docker only**, and there is no `dnx` command for it.

For a security review, a single pinned image is easier to reason about than a package plus a runtime plus native dependencies: the contents are fixed, the digest is verifiable, and there is no SDK on the host to keep patched.

```bash
docker run --rm -i \
  -v /srv/documents:/data \
  -v /srv/licenses:/license:ro \
  -e GROUPDOCS_MCP_STORAGE_PATH=/data \
  -e GROUPDOCS_LICENSE_PATH=/license/GroupDocs.Parser.lic \
  ghcr.io/groupdocs-parser/parser-net-mcp:26.9.0
```

* **Pin the tag** (`:26.9.0`, not `:latest`) so a new release cannot change behaviour underneath a running workflow.
* **Mount deliberately** — the container sees exactly the folders you give it, and nothing else. That is a tighter boundary than a process running as your user.
* **Licence read-only.**

## The part that still travels

Extraction produces **document content as text**, and that content goes into the conversation. With a cloud-hosted model, *"the invoice total is £84,000 and the client is Acme Holdings"* has reached your model provider — the document did not move, but its contents did.

For sensitive corpora, that is the argument for a locally-hosted model. For everything else it is at least worth knowing, and worth phrasing prompts accordingly: *"tell me which files have totals over £50,000, not the totals themselves"* is a request an agent will honour.

## License management

* **License file** — read from local disk by the local container. Fully offline; the right answer for air-gapped deployments.
* **Metered (pay-per-use)** — reports *usage* to GroupDocs servers, so it needs outbound egress. Document content is never part of that report.

Both are covered in [Licensing]({{< ref "parser/mcp/getting-started/licensing.md" >}}).

## What this fits — honestly

**A good fit:** invoice and form processing, document triage, barcode-driven workflows over scanned batches, and any of those inside a network that does not allow document uploads.

**Not what this is:** an OCR engine or a document-understanding service. It reads text layers, tables, metadata, images, and codes. Recognising printed text in scans, classifying documents, and extracting named fields by meaning are steps you build on top.

## FAQ

**Does any document content leave the machine?** Not from the server. Extracted content the agent reports back travels in the conversation to your model provider.

**Does it need internet at runtime?** No — only to pull the image, and when metered licensing is enabled.

**Can I run it air-gapped?** Yes: pre-pull the image, use a license file, pin the tag.

**What ports does it open?** None. stdio only.

**How do I prove that?** The [verification script]({{< ref "parser/net/mcp/troubleshooting.md" >}}#verifying-an-installation-end-to-end) performs a real handshake and a real engine call so you can watch exactly what happens.

## Read more

* [3 architectures for AI document processing, and the one that keeps files inside your network](https://blog.groupdocs.com/total/on-premise-ai-document-processing-mcp/) — the same scenario on the GroupDocs blog, with worked examples and the trade-offs.

