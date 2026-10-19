# GroupDocs.Parser Docs - AGENTS.md

> Instructions for AI agents working with **GroupDocs.Parser** documentation in this repository.

GroupDocs.Parser is an on-premise SDK for extracting text, formatted text, tables, images, metadata, hyperlinks and barcodes from PDF, Microsoft Office, OpenDocument, email, eBook, archive and image files. The .NET and Python docs also cover extracting annotations.

**Supported formats (canon):** the per-platform tables below. Format lists differ between platforms, so do not quote one platform's list or count for another.

## Platforms in this docs tree

| Platform slug | Docs hub | Formats table |
|---|---|---|
| `net` | [net/_index.md](net/_index.md) | [net/getting-started/supported-document-formats.md](net/getting-started/supported-document-formats.md) |
| `java` | [java/_index.md](java/_index.md) | [java/getting-started/supported-document-formats.md](java/getting-started/supported-document-formats.md) |
| `python-net` | [python-net/_index.md](python-net/_index.md) | [python-net/supported-document-formats.md](python-net/supported-document-formats.md) |
| `nodejs-java` | [nodejs-java/_index.md](nodejs-java/_index.md) | [nodejs-java/getting-started/supported-document-formats.md](nodejs-java/getting-started/supported-document-formats.md) |
| `mcp` | [mcp/_index.md](mcp/_index.md) | [mcp/supported-formats.md](mcp/supported-formats.md) |

## .NET target frameworks (canon)

When documenting .NET install/runtime requirements, use: **net462** (.NET Framework 4.6.2 or later), **netstandard2.1**, **net6.0**, **net8.0**, **net10.0** (do not invent other TFMs). These are the target frameworks of the GroupDocs.Parser NuGet package (26.8.0).

The Python package (`groupdocs-parser-net`) embeds its own .NET runtime, so Python readers install no .NET at all. Its requirements are on the python-net system requirements page.

## Product lines (do not confuse)

| Line | Where to send readers |
|---|---|
| **On-premise SDK** (this docs tree) | Platform hubs above; examples repos; NuGet / Maven / PyPI / npm packages |
| **GroupDocs.Parser MCP server** (this docs tree, `mcp/` and `net/mcp/`) | An MCP server that extracts text, tables, images, metadata and barcodes for AI agents |
| **GroupDocs Cloud** | Separate Cloud product docs/packages - do not mix install snippets |
| **Docs MCP** | Link only: [https://docs.groupdocs.com/mcp](https://docs.groupdocs.com/mcp) - do not edit MCP repos from this surface |

## Editing conventions

- The public URL of a page comes from its `url:` front matter, not from its file path. Keep `url:` unchanged when you move or rename a file.
- `{{< ref "..." >}}` resolves content paths: after moving a file, update every `ref` that points to it.
- Redirects are Hugo `aliases:` front matter on the destination page.
- Menu titles (`title:`, `linkTitle:`) are plain text: no emoji.
- Code examples on a page have runnable counterparts in the platform's examples repository; keep the two in sync.
- The python-net input files in `python-net/_sample_files/` are generated in the GroupDocs.Parser-SampleFiles repository and synced here: change a sample there, not in this tree.

## Useful entry URLs

| Resource | URL |
|---|---|
| Docs home | [https://docs.groupdocs.com/parser/](https://docs.groupdocs.com/parser/) |
| Products | [https://products.groupdocs.com/parser/](https://products.groupdocs.com/parser/) |
| Releases | [https://releases.groupdocs.com/parser/](https://releases.groupdocs.com/parser/) |
| API reference | [https://reference.groupdocs.com/parser/](https://reference.groupdocs.com/parser/) |
| Examples (.NET) | [https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET) |
| Examples (Java) | [https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java) |
| Examples (Python) | [https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET) |
| MCP server | [https://github.com/groupdocs-parser/GroupDocs.Parser.Mcp](https://github.com/groupdocs-parser/GroupDocs.Parser.Mcp) |

Also see [llms.txt](llms.txt) in this repository root for a compact machine-oriented index.
