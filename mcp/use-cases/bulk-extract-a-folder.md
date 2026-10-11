---
id: mcp-uc-bulk-extract-a-folder
url: parser/mcp/use-cases/bulk-extract-a-folder
title: How to extract data from a folder of documents at once
linkTitle: Bulk-extract a folder
weight: 4
description: "Extract text, tables, or barcodes from many documents in one prompt with an AI agent over MCP, and get one consolidated result."
keywords: bulk document extraction AI, extract from folder of PDFs, batch table extraction MCP, process many documents agent
productName: GroupDocs.Parser MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to extract data from a folder of documents at once"
        description: "Extract text, tables, or barcodes from many documents in one prompt with an AI agent over MCP, and get one consolidated result."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Parser MCP server with Docker and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "For every PDF in my documents folder: extract the tables as JSON, total the amount column, and give me one table of file name → total → page count."
---

Extraction is rarely about one file. It is about the folder that arrived this morning.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server, which ships as a Docker image only. Installation and client setup: [MCP server for .NET]({{< ref "parser/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## Setup

Mount the folder into the container as `/data` and point `GROUPDOCS_MCP_STORAGE_PATH` at it ([configuration]({{< ref "parser/net/mcp/configuration.md" >}})). Files are resolved by name, so the agent passes `invoice-014.pdf`, not a host path.

## The prompt

> For every PDF in my documents folder: extract the tables as JSON, total the amount column, and give me one table of file name → total → page count.

The agent loops, calls [`extract_tables`]({{< ref "parser/mcp/tools-reference/extract-tables.md" >}}) and [`get_document_info`]({{< ref "parser/mcp/tools-reference/get-document-info.md" >}}) per file, and assembles the result.

## Triage first on a mixed folder

Not every file will cooperate. One cheap pass up front tells you what you are dealing with:

> First, for each file, report format, page count, and whether text extraction returns anything. Then extract only from the ones that do.

The files that return nothing are scans — handle them with [`extract_barcodes`]({{< ref "parser/mcp/tools-reference/extract-barcodes.md" >}}) or set them aside.

## Keep the run honest and affordable

* **Ask for failures explicitly.** A password-protected or corrupt file fails its own call; a batch that reports only successes hides them.
* **Restrict by page** where the data has a known location — the first page of each invoice, say.
* **Avoid [`extract_images`]({{< ref "parser/mcp/tools-reference/extract-images.md" >}}) in bulk** unless you want the images: it writes files per image, and a folder of illustrated reports produces hundreds.
* **Watch metered usage.** Under [metered licensing]({{< ref "parser/mcp/getting-started/licensing.md" >}}#metered-pay-per-use-licensing) every call is billed processing; a targeted sweep costs less than an exhaustive one.
* **Check the licence first.** Unlicensed extraction is limited, and a partial read across a whole folder is a quietly wrong dataset — [`get_license_status`]({{< ref "parser/mcp/tools-reference/get-license-status.md" >}}).

## Where the results go next

The agent holds structured data at this point, so the useful follow-ups are ordinary questions:

> Which three invoices have the highest totals?
> Which files have no barcode at all?
> Write the totals to a CSV in the output folder.

All of it from documents that never left the machine — [architecture]({{< ref "parser/mcp/use-cases/on-premise-document-extraction.md" >}}).

## Read more

* [Enforce consistent data capture with automated extraction workflows using MCP](https://blog.groupdocs.com/parser/enforce-consistent-data-capture-bulk-extraction-mcp/) — the same scenario on the GroupDocs blog, with worked examples and the trade-offs.

