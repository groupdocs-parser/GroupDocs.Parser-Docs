---
id: command-line-interface
url: parser/python-net/command-line-interface
title: Command Line Interface
linkTitle: Command Line
weight: 5
description: "Use the groupdocs-parser command to extract text, document info, metadata, images and annotations from documents in a shell, a Make rule or a CI step, without writing Python code."
keywords: groupdocs-parser, command line, CLI, extract text command line, extract images command line, python -m groupdocs.parser
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
---

Installing `groupdocs-parser-net` also installs the `groupdocs-parser` command. It runs the same engine as the Python API, so you can pull text, metadata, images and annotations out of documents from a shell, a Make rule or a CI step without writing a script. If the `Scripts` (Windows) or `bin` (Linux, macOS) folder of your Python is not on `PATH`, run `python -m groupdocs.parser` instead; it takes the same arguments.

```bash
groupdocs-parser --version
```

```bash
groupdocs-parser 26.10.0
```

## Commands

| Command | What it does |
| --- | --- |
| `text FILE` | Prints the text of the document, or of one page |
| `info FILE` | Prints the format, extension, page count and size |
| `metadata FILE` | Lists the metadata properties as `name = value` |
| `images FILE --out DIR` | Saves the embedded images as `image-001.png`, `image-002.jpeg`, ... |
| `annotations FILE` | Lists the annotations (comments, notes) with their page, type and author |
| `list-formats` | Lists every supported extension with its family and description |

Run `groupdocs-parser --help` or `groupdocs-parser <command> --help` for the full list of flags.

### Options

| Option | Commands | Meaning |
| --- | --- | --- |
| `--page N` | `text`, `images`, `annotations` | Only page `N`. Pages are counted from 1 on the command line; the Python API counts from 0 |
| `--format plain\|markdown\|html` | `text` | Plain text (default), Markdown or HTML |
| `--ocr` | `text` | Recognize the text of scanned pages and images (plain text only) |
| `--include-annotations` | `text` | Add the annotations' text to the output (plain text only) |
| `-o FILE`, `--output FILE` | `text` | Write the text to a UTF-8 file instead of the console |
| `--json` | `info`, `metadata`, `annotations` | Print JSON instead of text |
| `--out DIR` | `images` | Folder for the extracted images; created if missing |
| `--password PASSWORD` | every command that opens a file | Password of a protected document |
| `--license PATH` | all (before the command) | Apply a license file first. Without it, `GROUPDOCS_LIC_PATH` is used if set |

## Examples

```bash
groupdocs-parser text report.pdf                        # plain text to the console
groupdocs-parser text report.pdf --page 2 -o page2.txt  # one page, to a UTF-8 file
groupdocs-parser text report.docx --format markdown     # Markdown, or html
groupdocs-parser text scan.pdf --ocr                    # recognize text in scanned pages
groupdocs-parser text reviewed.docx --include-annotations
groupdocs-parser info report.pdf --json                 # format, pages, size
groupdocs-parser metadata report.docx                   # name = value
groupdocs-parser images report.pdf --out images/        # writes image-001.png, ...
groupdocs-parser annotations reviewed.docx --json       # comments with page, author and time
groupdocs-parser list-formats                           # every supported extension and family

groupdocs-parser --license license.lic text protected.pdf --password secret
```

`info` on a two-page PDF prints:

```text
format:    Portable Document Format File
extension: .pdf
pages:     2
size:      150567
```

and with `--json`:

```json
{
  "format": "Portable Document Format File",
  "extension": ".pdf",
  "pages": 2,
  "size": 150567
}
```

`list-formats` prints one line per file type:

```text
.7z      ARCHIVE          7z Archive File
.azw3    EBOOK            AZW3
.bmp     IMAGE            Bitmap Image File
.bz2     ARCHIVE          Bzip2 Archive File
.chm     EBOOK            Compiled HTML Help File
...
```

`metadata`, `images` and `annotations` print a count such as `# 9 properties` to standard error, so standard output carries only the data and can be piped.

## Exit codes

| Code | Meaning |
| --- | --- |
| `0` | Success |
| `2` | A user error: the file does not exist, or an option is wrong (for example `--page 0`) |
| `1` | An engine error, or a format that does not support the operation (for example `annotations` on a spreadsheet) |

Errors are printed to standard error as one line, for example `error: no such file: missing.pdf`. A protected document opened without `--password` adds the hint `-- pass --password`.

## Licensing

Without a license the command runs in evaluation mode: `text` stops after about 20 lines and `metadata` returns the first 5 properties (see [Licensing and evaluation limitations]({{< ref "parser/python-net/getting-started/licensing-and-subscription.md" >}})). Pass `--license PATH` or set the `GROUPDOCS_LIC_PATH` environment variable.

## When to use Python instead

The command covers the common one-document tasks. Use the Python API for:

- template-based parsing (`parse_by_template`) and form data (`parse_form`);
- search, highlights, text areas, tables, hyperlinks, barcodes and the table of contents;
- documents in memory (streams) and documents inside archives and emails (`get_container`);
- logging and the external-resource handler ([Logging]({{< ref "parser/python-net/developer-guide/advanced-usage/logging" >}}), [Handle loading of external resources]({{< ref "parser/python-net/developer-guide/advanced-usage/loading/handle-loading-of-external-resources" >}}));
- processing many documents in one process: the first document a process opens takes 20-30 seconds while the engine loads, and every `groupdocs-parser` run is a new process.
