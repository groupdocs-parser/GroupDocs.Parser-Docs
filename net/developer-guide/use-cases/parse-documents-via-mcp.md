---
id: parse-documents-via-mcp
url: /parser/net/use-cases/parse-documents-via-mcp/
title: 4 MCP Tool Mirror Architectures for Document Parsing - Technical Deep Dive
weight: 1
description: "Technical analysis of 4 MCP tool mirror architectures for document parsing using GroupDocs.Parser for .NET. Docker registration, get_document_info, extract_text, and extract_tables as Markdown."
keywords: GroupDocs.Parser, GroupDocs.Parser.Mcp, MCP server, Docker, extract_text, extract_tables, get_document_info, .NET, C#, document parsing, architecture, performance, scalability
productName: GroupDocs.Parser for .NET
structuredData:
    showOrganization: True
toc: true
draft: true
hideChildren: False
---

{{< alert style="info" >}}
💡 Full working example available on GitHub:
[parser-mcp-document-tools-dotnet](https://github.com/groupdocs-parser/parser-mcp-document-tools-dotnet)
{{< /alert >}}

## Executive Summary

Four offline verification architectures for MCP document tools are a
GroupDocs.Parser design for .NET that separates Docker registration from
in-process mirrors of `get_document_info`, `extract_text`, and `extract_tables`.
The MCP server ships Docker-only because the engine exceeds NuGet size limits,
yet teams still need CI proof that Parser calls match those tool contracts.
This page scores registration, info JSON, text extract, and Markdown tables in
one console host for performance, scalability, complexity, integration, and
operations.

{{< alert style="warning" >}}
**For Production Use:** Review the [complete implementation](https://github.com/groupdocs-parser/parser-mcp-document-tools-dotnet) and test in your environment before deployment. Pin `ghcr.io/groupdocs-parser/parser-net-mcp` tags (for example `:26.9.0`) instead of `:latest`.
{{< /alert >}}

## What This Guide Covers

The scope is
[parser-mcp-document-tools-dotnet](https://github.com/groupdocs-parser/parser-mcp-document-tools-dotnet):
`EnsureMcpServerConfig`, `GetDocumentInfoViaMcp`, `ExtractTextViaMcp`, and
`ExtractTablesAsMarkdownViaMcp`. Read this if you publish an MCP tool list and
need CI proof without a live handshake.

**Prerequisites:**
- .NET 8 SDK and GroupDocs.Parser 26.10.0
- Docker Engine when starting GroupDocs.Parser.Mcp; optional for local mirrors
- Sample DOCX/XLSX under `documents/` (`sample.docx`: 15214 bytes, 2 pages)

## Architecture Comparison Matrix

| Architecture | Complexity | Throughput | Latency | Scalability | Resource Efficiency |
|--------------|-----------|-----------|---------|------------|-------------------|
| **EnsureMcpServerConfig** | Low | N/A (config write) | Sub-second | Config replicas | High (JSON only) |
| **GetDocumentInfoViaMcp** | Low | High for metadata | Low | Horizontal workers | High |
| **ExtractTextViaMcp** | Medium | Bound by page text | Medium | Vertical memory | Medium |
| **ExtractTablesAsMarkdownViaMcp** | Medium-High | Bound by table count | Medium-High | Prefer batch hosts | Medium |

## Architecture 1: EnsureMcpServerConfig - Config Emission Pattern

### Design Overview

`EnsureMcpServerConfig` is the registration plane. It emits an `mcpServers`
JSON document that tells MCP clients to run `docker` with `--rm -i` and a bind
mount of the host documents folder at `/data`. The payload lists seven tool
names, including `get_document_info`, `extract_text`, and `extract_tables`, so
operators can audit the advertised surface before the container starts.

**Design Pattern:** Configuration-as-code emission  
**Concurrency Model:** Single-threaded write; safe once per deploy  
**State Management:** Stateless; UTF-8 file on disk

### Implementation Architecture

Build the Docker-backed `mcpServers` object:

```csharp
// JSON for MCP clients: docker run --rm -i -v <host>:/data <image>
string normalizedHost = hostDocumentsPath.Replace('\\', '/');
var payload = new
{
    mcpServers = new
    {
        groupdocs_parser = new
        {
            command = "docker",
            args = new[]
            {
                "run", "--rm", "-i",
                "-v", $"{normalizedHost}:/data",
                McpImage
            }
        }
    },
```

Add the advertised tools and pin-tag note, then write JSON:

```csharp
    tools = new[]
    {
        "extract_text",
        "extract_tables",
        "extract_metadata",
        "extract_images",
        "extract_barcodes",
        "get_document_info",
        "get_license_status"
    },
    notes = "Pin the image tag in shared configs "
        + "(for example :26.9.0) instead of :latest."
};
var json = JsonSerializer.Serialize(
    payload,
    new JsonSerializerOptions { WriteIndented = true });
File.WriteAllText(outputPath, json, Encoding.UTF8);
```

### Performance Profile

Config emission is a deploy-time filesystem write. Correct volume paths and
pinned tags matter more than CPU. See
[GroupDocs.Parser MCP](https://docs.groupdocs.com/parser/net/mcp/) for live
server setup.

### Scalability Analysis

**Horizontal Scaling:** Copy JSON per MCP client host.  
**Bottlenecks:** Wrong `/data` mounts and floating `:latest` tags.

### Operational Considerations

- Diff written JSON in CI when tool lists change
- Fail deploy if the host documents path is missing
- Pin image version tags; avoid silent retags

## Architecture 2: GetDocumentInfoViaMcp - Metadata Snapshot Pattern

### Design Overview

`GetDocumentInfoViaMcp` mirrors the MCP `get_document_info` tool. It opens a
`Parser`, reads `GetDocumentInfo`, and writes `fileName`, `fileType`,
`pageCount`, and `sizeBytes` as JSON. I used this path first when validating
that `sample.docx` still reported 2 pages and 15214 bytes after packaging
changes, because metadata failures are cheaper to diagnose than full-text
regressions.

**Design Pattern:** Synchronous metadata snapshot  
**Concurrency Model:** One parser instance per call  
**State Management:** Ephemeral; only the JSON file persists

### Implementation Architecture

```csharp
using var parser = new Parser(sourcePath);
var info = parser.GetDocumentInfo();

var payload = new
{
    fileName = Path.GetFileName(sourcePath),
    fileType = info?.FileType?.ToString(),
    pageCount = info?.PageCount,
    sizeBytes = info?.Size
};

File.WriteAllText(
    outputPath,
    JsonSerializer.Serialize(
        payload,
        new JsonSerializerOptions { WriteIndented = true }),
    Encoding.UTF8);
```

### Performance Profile

Latency stays low because only summary fields are read. Use it as a preflight
gate before `extract_text`, keeping JSON fields aligned with the MCP tool shape.

### Scalability Analysis

**Horizontal Scaling:** Shard files across workers; each call is independent.  
**Bottlenecks:** Network filesystem latency when sources are remote.

### Operational Considerations

- Assert `pageCount` and `sizeBytes` against golden files
- Null-conditional fields avoid hard fails on sparse info
- Retry only on IO; do not retry unsupported formats blindly

## Architecture 3: ExtractTextViaMcp - Streaming Text Pattern

### Design Overview

`ExtractTextViaMcp` mirrors `extract_text` using `Parser.GetText`. A null
reader becomes an empty file and a zero character count, which keeps MCP client
loops honest when a format has no text layer. The console treats a positive
character count as the process success signal.

**Design Pattern:** Stream-to-disk extract  
**Concurrency Model:** Synchronous read on a single thread  
**State Management:** Full text buffered then written UTF-8

### Implementation Architecture

```csharp
using var parser = new Parser(sourcePath);
using TextReader? reader = parser.GetText();
if (reader == null)
{
    File.WriteAllText(outputPath, string.Empty, Encoding.UTF8);
    return 0;
}

string text = reader.ReadToEnd();
File.WriteAllText(outputPath, text, Encoding.UTF8);
return text.Length;
```

### Performance Profile

Throughput tracks page count; latency rises because `ReadToEnd` materializes
the full extract matching the default MCP `extract_text` contract. Use the
mirror in CI; the Docker server remains the production tool host.

### Scalability Analysis

**Horizontal Scaling:** Partition document queues per worker.  
**Bottlenecks:** Peak memory on multi-megabyte text dumps.

### Operational Considerations

- Log returned character counts per file
- Empty output is a soft failure (exit code 2 in `Main`)
- Dispose the reader and parser via `using`

## Architecture 4: ExtractTablesAsMarkdownViaMcp - Dual-Path Markdown Pattern

### Design Overview

`ExtractTablesAsMarkdownViaMcp` mirrors `extract_tables`, which returns
Markdown by default for chat rendering. The preferred path is
`GetFormattedText` with `FormattedTextMode.Markdown`. When that path is
unavailable, the method checks `Features.Tables` and builds pipe tables from
`GetTables`. The sample XLSX run writes Sheet1 and Sheet2 Markdown under
`Result/extract-tables.md`.

**Design Pattern:** Prefer-formatted, fallback-structural  
**Concurrency Model:** Synchronous; one document at a time  
**State Management:** Markdown string or `StringBuilder` then disk write

### Implementation Architecture

Prefer formatted Markdown and count pipe rows as a progress signal:

```csharp
using var parser = new Parser(sourcePath);

using (TextReader? formatted = parser.GetFormattedText(
           new FormattedTextOptions(FormattedTextMode.Markdown)))
{
    if (formatted != null)
    {
        string markdown = formatted.ReadToEnd();
        File.WriteAllText(outputPath, markdown, Encoding.UTF8);
        int rows = 0;
        using var sr = new StringReader(markdown);
        string? line;
        while ((line = sr.ReadLine()) != null)
        {
            if (line.Contains('|', StringComparison.Ordinal))
                rows++;
        }
        return rows > 0
            ? rows
            : (string.IsNullOrWhiteSpace(markdown) ? 0 : 1);
    }
}
```

Fall back when formatted Markdown is missing:

```csharp
if (!parser.Features.Tables)
{
    File.WriteAllText(
        outputPath,
        "_Tables extraction is not supported for this file._\n",
        Encoding.UTF8);
    return 0;
}

IEnumerable<PageTableArea>? tables = parser.GetTables();
if (tables == null)
{
    File.WriteAllText(outputPath, "_No tables returned._\n", Encoding.UTF8);
    return 0;
}
```

### Performance Profile

Formatted Markdown is cheaper to ship to chat UIs than rebuilding cells.
Structural `GetTables` grows with row and column counts - prefer XLSX when
table fidelity matters, and keep Markdown aligned with `extract_tables`.

### Scalability Analysis

**Horizontal Scaling:** Batch spreadsheets across workers.  
**Bottlenecks:** Cell escaping and large `StringBuilder` growth.

### Operational Considerations

- Track returned row counts and empty Markdown markers
- Explicit `_No tables returned._` messages aid operators
- Escape `|` in cell text before joining pipe rows

## How do I choose between Docker MCP and local Parser mirrors?

Choose Docker MCP when MCP clients must call named tools against files under
`/data` (see [MCP troubleshooting](https://docs.groupdocs.com/parser/net/mcp/troubleshooting/)).
Choose local Parser mirrors when CI must verify `get_document_info`,
`extract_text`, and `extract_tables` without a live handshake. Most teams keep
both: config for registration, mirrors for offline proof, then a pinned
`parser-net-mcp` image on production MCP client hosts.

## When to use this approach

Use these architectures when GroupDocs.Parser.Mcp must run as Docker-only MCP
and your .NET service still needs offline verification of tool-equivalent APIs.
Skip MCP registration only if you call GroupDocs.Parser solely in-process and
never expose tools to MCP clients.

## Common pitfalls

- Leaving `:latest` instead of pinning tags such as `:26.9.0`
- Mounting the wrong host folder so tools see empty `/data`
- Expecting a NuGet MCP package - the server is Docker-only
- Treating a null `GetText` reader as an exception instead of empty output
- Building pipe tables without escaping `|` in cell text

## Architecture Decision Records

### ADR 1: Docker-only MCP server

GroupDocs.Parser.Mcp is a container image because package size exceeds NuGet
limits. Local demos verify tool-equivalent APIs without a live MCP handshake.

### ADR 2: Dual verification path

Keep config emission and library mirrors in one repository so registration and
behavior stay reviewable together.

## Notes

Install with `dotnet add package GroupDocs.Parser --version 26.10.0`. Start the
MCP server via Docker from the generated `mcpServers` entry, not NuGet. Layout:
`Program.cs`, `ParseViaMcp.csproj`, `documents/`, and generated `Result/`.

## FAQ

### Does the sample need Docker to produce Result files?

No. The console uses GroupDocs.Parser locally to write `document-info.json`,
`extract-text.txt`, and `extract-tables.md`. Docker is required only when an
MCP client starts GroupDocs.Parser.Mcp from the generated registration JSON.

### Which tools does the registration JSON advertise?

It lists `extract_text`, `extract_tables`, `extract_metadata`,
`extract_images`, `extract_barcodes`, `get_document_info`, and
`get_license_status`. The demo mirrors the first two plus `get_document_info`.

### Why is Markdown the table output format?

The MCP `extract_tables` tool returns Markdown by default for chat rendering,
so local mirrors stay consistent with MCP client UIs and the Docker server.

## Conclusion and Recommendations

Emit MCP config, snapshot document info, extract text, then emit Markdown
tables. Pin container tags, mount only the documents folder at `/data`, and keep
offline mirrors in CI so MCP clients keep a stable tool surface without a live
handshake for regressions.

**For Production Deployment:**
- Review the [complete source code](https://github.com/groupdocs-parser/parser-mcp-document-tools-dotnet)
- Test performance locally and monitor character/row counts
- Restrict volume mounts and keep licenses out of the published tree

## See Also

- [In-depth blog article about parse documents via MCP on .NET](https://blog.groupdocs.com/parser/parse-documents-via-mcp-net/)
- [GroupDocs.Parser MCP documentation](https://docs.groupdocs.com/parser/net/mcp/)
- [MCP troubleshooting](https://docs.groupdocs.com/parser/net/mcp/troubleshooting/)
- [Product documentation](https://docs.groupdocs.com/parser/net/)
- [API Reference](https://reference.groupdocs.com/parser/net/)
- [GitHub repository](https://github.com/groupdocs-parser/parser-mcp-document-tools-dotnet)
