---
id: mcp-uc-extract-tables-into-structured-data
url: parser/mcp/use-cases/extract-tables-into-structured-data
title: How to extract tables from documents into structured data
linkTitle: Tables to structured data
weight: 2
description: "Extract tables from PDFs and Office documents with an AI agent over MCP, as Markdown for review or JSON rows for processing."
keywords: extract table from PDF AI, document table to JSON, markdown table extraction agent, invoice line items extraction
productName: GroupDocs.Parser MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to extract tables from documents into structured data"
        description: "Extract tables from PDFs and Office documents with an AI agent over MCP, as Markdown for review or JSON rows for processing."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Parser MCP server with Docker or dnx and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "Extract the tables from invoice.pdf."
---

Tables are where the numbers live: invoice line items, price lists, test results, financial summaries. [`extract_tables`]({{< ref "parser/mcp/tools-reference/extract-tables.md" >}}) gets them out in the shape you need.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server, which ships as a Docker image only. Installation and client setup: [MCP server for .NET]({{< ref "parser/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## Markdown, to look at

> Extract the tables from invoice.pdf.

The default `format: "markdown"` returns real Markdown tables, which render immediately in chat, VS Code, and GitHub. When the point is *review* — does this look right? — this is the format that gets you there fastest.

## JSON, to compute with

> Extract the line items as JSON and total the amount column.

`format: "json"` returns structured rows. Now the agent can sum, compare, filter, and cross-check against another system, rather than reading numbers out of rendered text.

The rule: **Markdown when a human reads it next, JSON when code does.**

## Narrow it down

> Extract the table on page 4.

Table detection across a long document is unnecessary work when you know where the table is. `page` restricts it — faster, and under [metered licensing]({{< ref "parser/mcp/getting-started/licensing.md" >}}#metered-pay-per-use-licensing) cheaper.

## A realistic invoice flow

> For each invoice in the folder: extract the line-items table as JSON, total it, and compare the total against the number in the text. Flag any mismatch.

That is two tools ([`extract_tables`]({{< ref "parser/mcp/tools-reference/extract-tables.md" >}}) and [`extract_text`]({{< ref "parser/mcp/tools-reference/extract-text.md" >}})) and arithmetic the agent does itself — a reconciliation pass over a folder, without a single upload.

## What to watch

* **Scans have no tables to find.** No text layer means no table structure; the extraction comes back empty. That is the file, not the tool.
* **Complex layouts vary.** Merged cells, nested tables, and multi-page tables do not always survive as cleanly as a simple grid. Spot-check one document before trusting a batch of two hundred.
* **Verify totals, do not assume them.** When the arithmetic matters, have the agent show the rows it summed.
