---
id: agents-and-llm-integration
url: parser/python-net/agents-and-llm-integration
title: Agents and LLM Integration
linkTitle: Agents and LLMs
description: "GroupDocs.Parser for Python via .NET is AI agent and LLM friendly: machine-readable documentation, MCP servers, AGENTS.md shipped inside the pip package, a command line, and runnable examples that turn documents into data for AI pipelines."
weight: 9
keywords: AI, LLM, agent, MCP, machine-readable, documentation, Claude, GPT, Copilot, AGENTS.md, RAG, document extraction, text extraction, table extraction
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
---

## AI agent and LLM friendly

GroupDocs.Parser for Python via .NET is designed to work with AI agents, LLMs, and code generation tools. The library ships machine-readable documentation in several formats, including an `AGENTS.md` file inside the pip package itself, so that AI assistants can discover and use the API without manual guidance.

## MCP server for the documentation

GroupDocs provides an [MCP (Model Context Protocol) server](https://docs.groupdocs.com/mcp) that lets LLMs query the documentation on demand instead of loading it all at once. This saves tokens and lets your AI assistant fetch only what it needs for the current task.

To connect your AI tool to the MCP server, add the GroupDocs endpoint to your MCP configuration:

{{< tabs "mcp-setup">}}
{{< tab "Claude Code / Claude Desktop" >}}
```json
// Claude Code:    ~/.claude/settings.json (or project .mcp.json)
// Claude Desktop: ~/Library/Application Support/Claude/claude_desktop_config.json
{
  "mcpServers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< tab "GitHub Copilot" >}}
```json
// .vscode/mcp.json in your project root
{
  "servers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< tab "Cursor" >}}
```json
// .cursor/mcp.json in your project root
{
  "mcpServers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< tab "Generic MCP" >}}
```json
// Any MCP-compatible client
{
  "mcpServers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< /tabs >}}

See [https://docs.groupdocs.com/mcp](https://docs.groupdocs.com/mcp) for full setup instructions and the list of available tools.

## MCP server for extracting data

The [GroupDocs.Parser MCP server]({{< ref "parser/mcp/_index.md" >}}) is a separate server that does the work itself: it lets an AI agent extract text, tables, metadata, images and barcodes from documents, locally on your machine. The server is the .NET build of GroupDocs.Parser and ships as a Docker image, so it runs alongside a Python project without a .NET installation.

Use the documentation MCP server when an agent writes Python code against this library, and the extraction MCP server when the agent itself reads documents. [SDK, Cloud or MCP?]({{< ref "parser/python-net/sdk-cloud-mcp.md" >}}) compares the options.

## AGENTS.md: built into the package

The `groupdocs-parser-net` pip package includes an `AGENTS.md` file at `groupdocs/parser/AGENTS.md`. AI coding assistants that scan installed packages (such as Claude Code, Cursor, and GitHub Copilot) can read it to learn the imports, key patterns, callbacks, the command line, licensing, platform requirements, and troubleshooting tips. Its full text is reproduced [below](#agentsmd-reference).

After installing the package, you can find it with:

```bash
pip show -f groupdocs-parser-net | grep AGENTS
```

## Machine-readable documentation

Every documentation page is available as a plain Markdown file that AI tools can fetch and process directly:

| Resource | URL |
|---|---|
| Full documentation (single file) | `https://docs.groupdocs.com/parser/python-net/llms-full.txt` |
| Full documentation (all products) | `https://docs.groupdocs.com/llms-full.txt` |
| Individual page (any page) | Append `.md` to the page URL |
| Quick start guide | `https://docs.groupdocs.com/parser/python-net/quick-start.md` |

### How to use with AI tools

Point your AI assistant to the full documentation file for comprehensive context:

```
Fetch https://docs.groupdocs.com/parser/python-net/llms-full.txt and use it
as a reference for GroupDocs.Parser for Python via .NET API.
```

## Why GroupDocs.Parser is a good building block for AI pipelines

Models read text, not files. GroupDocs.Parser turns the documents an agent receives into text and data it can use, with the same API across PDF, Office, OpenDocument, email, eBook, archive and image files:

- **Feed RAG and search**: extract plain text, or Markdown that keeps headings, lists and tables, page by page.
- **Get structured data**: read tables as rows and cells, metadata as names and values, and invoice-like layouts with templates.
- **Read what reviewers said**: collect comments and notes with their authors and pages, or include them in the extracted text.
- **Open what is inside**: process the documents inside ZIP archives, email attachments and PDF portfolios.
- **Recognize scans**: run OCR on scanned pages and images.

A typical extraction step looks like this:

{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
import json

from groupdocs.parser import Parser

with Parser("invoice.pdf") as parser:
    info = parser.get_document_info()
    text = parser.get_text()
    tables = parser.get_tables() or []
    record = {
        "format": info.file_type.file_format,
        "pages": info.page_count,
        "characters": len(text or ""),
        "tables": [
            [[table[row, column].text for column in range(table.column_count)]
             for row in range(table.row_count)]
            for table in tables
        ],
    }
    # feed `record` into your index, an agent's context, or a review queue
    print(json.dumps(record, indent=2))
```

{{< /tab >}}
{{< tab "invoice.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [invoice.pdf](/parser/python-net/_sample_files/agents-and-llm-integration/invoice.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

For the sample invoice it prints the document's size and its three tables (abridged):

```text
{
  "format": "Portable Document Format File",
  "pages": 1,
  "characters": 1714,
  "tables": [
    [
      [
        "Invoice Number",
        "INV-3337"
      ],
      ...
```

An agent with a shell can get the same data without writing code, through the [command line]({{< ref "parser/python-net/developer-guide/command-line-interface" >}}): `groupdocs-parser text invoice.pdf --format markdown`, `groupdocs-parser info invoice.pdf --json`, `groupdocs-parser annotations reviewed.docx --json`.

For end-to-end examples, see the [Developer Guide]({{< ref "parser/python-net/developer-guide/_index.md" >}}). Every code example has a runnable counterpart in the [examples repository](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET).

{{< alert style="info" >}}
Without a license, extracted text stops after about 20 lines and only the first 5 metadata items are returned. Set the `GROUPDOCS_LIC_PATH` environment variable before an agent runs these steps. See [Licensing and evaluation limitations]({{< ref "parser/python-net/licensing-and-evaluation-limitations.md" >}}).
{{< /alert >}}

## Pair with other GroupDocs products

For richer AI document pipelines, chain GroupDocs.Parser with:

- [GroupDocs.Conversion for Python via .NET](https://docs.groupdocs.com/conversion/python-net/): convert legacy and exotic formats before extraction.
- [GroupDocs.Viewer for Python via .NET](https://docs.groupdocs.com/viewer/python-net/): render pages to PNG or HTML for vision models and human review.
- [GroupDocs.Metadata for Python via .NET](https://docs.groupdocs.com/metadata/python-net/): edit or remove the metadata Parser reads.

## See also

- [Quick Start Guide]({{< ref "parser/python-net/getting-started/quick-start.md" >}}): your first extraction script in five minutes
- [Developer Guide]({{< ref "parser/python-net/developer-guide/_index.md" >}}): runnable examples for every extraction feature
- [API Reference](https://reference.groupdocs.com/parser/python-net): full class and method documentation
- [SDK, Cloud or MCP?]({{< ref "parser/python-net/sdk-cloud-mcp.md" >}}): which GroupDocs.Parser product line fits your project

## AGENTS.md reference

The complete `AGENTS.md` shipped in the 26.10 package:

````markdown
# GroupDocs.Parser for Python via .NET -- AGENTS.md

> Instructions for AI agents working with this package.

Extract text, formatted text (HTML, Markdown), tables, images, metadata, hyperlinks, barcodes, annotations and the table of contents from PDF, Word, Excel, PowerPoint, email, eBook, archive and image files (60+ formats).

## Install

```bash
pip install groupdocs-parser-net
```

**Python**: 3.5 - 3.14 | **Platforms**: Windows x64, Linux x64 (glibc 2.27+), macOS 13+ (Intel, Apple Silicon; OCR needs 13.3+)

## Resources

| Resource | URL |
|---|---|
| Documentation | https://docs.groupdocs.com/parser/python-net/ |
| LLM-optimized docs | https://docs.groupdocs.com/parser/python-net/llms-full.txt |
| API reference | https://reference.groupdocs.com/parser/python-net/ |
| Code examples | https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET |
| Release notes | https://releases.groupdocs.com/parser/python-net/release-notes/ |
| PyPI | https://pypi.org/project/groupdocs-parser-net/ |
| Free support forum | https://forum.groupdocs.com/c/parser/ |
| Temporary license | https://purchase.groupdocs.com/temporary-license |

## MCP Server

If your environment has MCP configured, you can connect your AI tool to the GroupDocs documentation server for on-demand API lookups:

```json
{
  "mcpServers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```

Works with Claude Code (`~/.claude/settings.json`), Cursor (`.cursor/mcp.json`), VS Code Copilot (`.vscode/mcp.json`), and any MCP-compatible client. If MCP is unavailable, fall back to the LLM-optimized docs URL above and to this file, which is shipped inside the wheel.

## Imports

Each class lives in one module; the root exports only `Parser`, `License`, `Metered` and `Resources`. The exceptions are in `groupdocs.parser.exceptions`.

```python
from groupdocs.parser import License, Parser
from groupdocs.parser.data import AnnotationItem, PageImageArea, PageTableArea, Rectangle, Point, Size, ...
from groupdocs.parser.export import JsonExporter, XmlExporter
from groupdocs.parser.options import (FileType, FormattedTextMode, FormattedTextOptions, LoadOptions,
                                      PageAreaOptions, ParserSettings, PreviewOptions, TextOptions, ...)
from groupdocs.parser.templates import Template, TemplateField, TemplateTable, TemplateTableLayout, ...
from groupdocs.parser.exceptions import InvalidPasswordException, UnsupportedDocumentFormatException
```

## Extract Text

```python
from groupdocs.parser import Parser

with Parser("document.pdf") as parser:
    text = parser.get_text()          # str, or None when the format has no text
    if text is not None:
        print(text)
```

- `get_text()` / `get_formatted_text()` return a `str` that also answers `read_to_end()` -- 25.12 code and .NET-style code both work. `None` means not supported.
- One page: `parser.get_text(page_index)` (0-based). Options: `parser.get_text(TextOptions(True))` for raw mode, `TextOptions(False, True)` for OCR.
- Markdown or HTML: `parser.get_formatted_text(FormattedTextOptions(FormattedTextMode.MARKDOWN))`.

## Other Extraction

```python
from groupdocs.parser import Parser

with Parser("document.pdf") as parser:
    info = parser.get_document_info()                     # file_type.file_format, page_count, size
    for table in parser.get_tables() or []:               # PageTableArea
        rows = [[table[r, c].text for c in range(table.column_count)] for r in range(table.row_count)]
    for image in parser.get_images() or []:               # PageImageArea
        image.save("img%s" % image.file_type.extension)   # or image.get_image_stream().read() -> bytes
    for note in parser.get_annotations() or []:           # AnnotationItem: value, author_name, page_index
        print(note.author_name, note.value.strip())
    for item in parser.get_metadata() or []:              # MetadataItem: name, value
        print(item.name, item.value)
```

Check `parser.features.<name>` (text, tables, images, hyperlinks, annotations, metadata, structure, toc, ...) before a call when the format is unknown; an unsupported call returns `None`.

## Callbacks

```python
import io

from groupdocs.parser.options import ParserSettings, PreviewOptions

settings = ParserSettings(lambda level, message, exception: print(level, message))       # logger
settings = ParserSettings(external_resource_handler=lambda args: setattr(args, "skipped", True))
pages = {}
options = PreviewOptions(lambda page_number: pages.setdefault(page_number, io.BytesIO()))  # page previews
options.preview_page_render = lambda info: print("rendering page", info.page_number)
```

- `ParserSettings` has no empty constructor: pass a logger, an OCR connector or a resource handler (keywords pick which).
- The logger may be an object with `trace`/`warning`/`error`; the resource handler an object with `on_loading(args)` or a subclass of `ExternalResourceHandler`. The engine consults the handler for images in HTML and MHTML only.

## Command Line

`groupdocs-parser text|info|metadata|images|annotations|list-formats FILE` (or `python -m groupdocs.parser`). `text` takes `--page N` (from 1), `--format plain|markdown|html`, `--ocr`, `--include-annotations`, `-o FILE`; `info`, `metadata` and `annotations` take `--json`; `images` takes `--out DIR`; every command takes `--password`. Exit codes: 0 ok, 2 user error, 1 engine error or unsupported operation.

## Licensing

```python
from groupdocs.parser import License

# From file
License().set_license("path/to/license.lic")

# From stream
with open("license.lic", "rb") as f:
    License().set_license(f)
```

Or auto-apply: `export GROUPDOCS_LIC_PATH="path/to/license.lic"`

**Evaluation vs licensed.** Without a license the library still runs, but extracted text stops after about 20 lines and ends with "Evaluation only. Created with GroupDocs.Parser for .NET.", and only the first 5 metadata items are returned. Set `GROUPDOCS_LIC_PATH` (or call `License().set_license(...)`) and re-run to clear both. A 30-day full license is free: https://purchase.groupdocs.com/temporary-license

## API Reference

### Parser

| Method | Returns | Description |
|---|---|---|
| `Parser(path_or_stream, load_options=None, parser_settings=None)` | | Open a file path or a binary stream |
| `get_text(options=None, page_index=None)` | `str` or `None` | Plain text |
| `get_formatted_text(options, page_index=None)` | `str` or `None` | HTML / Markdown / plain |
| `get_document_info()` | `DocumentInfo` | Format, pages, size |
| `search(keyword, options=None)` | list of `SearchResult` | Keyword or regex search |
| `get_highlight(position, is_direct, options)` | `HighlightItem` | Text around a position |
| `get_structure()` | `XmlReader` | Document structure as XML |
| `get_toc()` | list of `TocItem` | Table of contents; `item.extract_text()` returns the section text |
| `get_container()` | list of `ContainerItem` | Archive entries / attachments |
| `get_metadata()` | list of `MetadataItem` | Document properties |
| `get_text_areas()` / `get_images()` / `get_hyperlinks()` / `get_barcodes()` / `get_tables()` | lists | Page areas, whole document or `(page_index, options)` |
| `get_annotations(page_index=None)` | list of `AnnotationItem` | Comments and notes |
| `get_page_preview(page_index)` | `Stream` (`.read()` -> bytes) | PNG preview of one page |
| `generate_preview(preview_options)` | `None` | Previews through callbacks |
| `parse_by_template(template)` | `DocumentData` | Fields and tables by layout |
| `parse_form()` | `DocumentData` | PDF form fields |
| `Parser.get_file_info(path_or_stream)` | `FileInfo` | static: format and encryption, without opening |

## Key Patterns

- **Properties**: use `snake_case` -- auto-mapped to .NET `PascalCase`. Assigning a misspelled property raises `AttributeError`.
- **Context managers**: `with Parser(...) as x:` ensures resources are released
- **Streams**: pass `open("file", "rb")` or `io.BytesIO(data)` where .NET expects Stream; a Stream the engine returns has `.read()`
- **Truth tests**: an engine object is true; a returned list is false when empty. Prefer `is None` checks for "not supported"
- **Enums**: case-insensitive, lazy-loaded (e.g., `FileType.DOCX`, `FormattedTextMode.MARKDOWN`)
- **Collections**: `for item in result`, `len(result)` and `result[i]` work on returned lists
- **Errors**: engine errors raise `GroupDocsParserException` or a subclass (`InvalidPasswordException`, `CorruptedDocumentException`, `UnsupportedDocumentFormatException`) from `groupdocs.parser.exceptions`, not `RuntimeError` as in 25.12

## Platform Requirements

| Platform | Requirements |
|---|---|
| Windows | None |
| Linux | `apt install libicu-dev libfontconfig1 fonts-liberation libgdiplus` (no Microsoft core fonts) |
| macOS | macOS 13 or newer; OCR (text from scanned pages and images) needs 13.3 or newer. `brew install mono-libgdiplus` |

libgdiplus serves presentations: page previews, ODP and OTP files, and PowerPoint files with tables or hyperlinks. Every other format works without it.

## Troubleshooting

**The process aborts with `Couldn't find a valid ICU package`** -- install ICU: `sudo apt install libicu-dev`. Do NOT set `DOTNET_SYSTEM_GLOBALIZATION_INVARIANT`.

**Merged table cells come out as separate rows in `get_structure()`** -- install the Liberation fonts: `sudo apt install fonts-liberation`

**`The type initializer for 'Gdip' threw an exception` / `DllNotFoundException: libgdiplus`** -- install libgdiplus: `sudo apt install libgdiplus` (macOS: `brew install mono-libgdiplus`). Presentations need it: page previews, ODP and OTP files, and PowerPoint files with tables or hyperlinks.

**The first `Parser(...)` takes 20-30 seconds** -- expected: the engine loads once per process; later documents open in a fraction of a second.

**An `aspose_data` folder of about 80 MB appears in the working directory** -- the first OCR call unpacks the recognition models there (no download). Set `Resources.storage_path = "/path/to/cache"` (`from groupdocs.parser import Resources`) before the first OCR call to choose the folder, e.g. when the working directory is read-only.

**`TypeError: __init__() - no overload matched` for `ParserSettings()`** -- there is no empty `ParserSettings`; pass a logger, OCR connector or resource handler.

**`is not a supported wheel on this platform`** -- the OS is older than glibc 2.27 / macOS 13, or pip is older than 20.3: `python -m pip install --upgrade pip`

**OCR, or text from an image, raises `GroupDocsParserException` naming `Microsoft.ML.OnnxRuntime.NativeMethods` / `DllNotFoundException` on macOS 13.0-13.2** -- OCR's ONNX Runtime library needs macOS 13.3 or newer; update macOS. Every other feature works on 13.0.

**`XType: com.apple.fonts is not accessible.` on stderr (macOS, in CI or a background service)** -- harmless: with no logged-in desktop session macOS reads the system font folders directly. Nothing to fix.

**`TypeLoadException`** -- reinstall: `pip install --force-reinstall groupdocs-parser-net`

**Still stuck?** Post your question at https://forum.groupdocs.com/c/parser/ -- the development team responds directly.
````
