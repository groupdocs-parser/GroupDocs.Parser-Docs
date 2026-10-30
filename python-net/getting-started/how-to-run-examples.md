---
id: how-to-run-examples
url: parser/python-net/how-to-run-examples
title: How to Run Examples
weight: 7
description: "How to clone, set up, and run the GroupDocs.Parser for Python via .NET example suite - locally, with a license, or in Docker."
keywords: "groupdocs parser python, examples, run examples, docker, ci"
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
structuredData:
  showOrganization: True
  application:
    name: Document Parser Tool
    description: The product extracts text, images, metadata, tables, and structured data from PDF, Word, Excel, PowerPoint and 65+ other file formats. Parser API also supports template-based extraction and container processing.
    productCode: parser
    productPlatform: python-net
  howTo:
    name: How to run Parser examples
    description: Learn how to run GroupDocs.Parser examples using Python
    steps:
      - name: Clone repository
        text: Clone the repository with Parser examples from GitHub
      - name: Install package
        text: Install the GroupDocs.Parser package with pip
      - name: Run examples
        text: Execute run_all_examples.py to run the examples
---

The complete project [GroupDocs.Parser for Python via .NET](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET) with code examples and sample files is hosted on GitHub. Every example of this documentation is there as a script, next to its input files.

## Prerequisites

- [Python 3.6-3.14](https://www.python.org/downloads/), 64-bit. The package itself supports Python 3.5, but the examples use f-strings, which need 3.6.
- On Linux, ICU, fontconfig, the Liberation fonts and `libgdiplus` (`libicu-dev libfontconfig1 fonts-liberation libgdiplus`). On macOS: `mono-libgdiplus` (via Homebrew). See [System Requirements]({{< ref "parser/python-net/getting-started/system-requirements.md" >}}) for what each one is needed for.

## Get the code

```bash
git clone https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET.git
cd ./GroupDocs.Parser-for-Python-via-.NET/Examples
```

## Project structure

```
Examples/
├── run_all_examples.py     # Runs every example and prints a pass/fail summary
├── _run_example.py         # Per-example wrapper (applies the license, reports evaluation-limit errors)
├── requirements.txt
├── product-overview/       # The quick examples of the product overview
├── licensing/              # Set a license from a file, a stream, or a metered key
├── getting-started/
│   ├── hello-world/        # Extract the text of a document
│   └── quick-start-guide/  # Extract text, metadata and tables
└── developer-guide/
    ├── basic-usage/        # Text, images, metadata, document info, templates, containers
    └── advanced-usage/     # Loading, extraction modes, tables, images, hyperlinks, annotations, logging
```

The folders follow this documentation: each page's examples are in the folder of that page, with the files they read.

## Set up

```bash
pip install -r requirements.txt
```

Until PyPI carries `groupdocs-parser-net` 26.10.0, that command finds no matching version. First install the 26.10 wheel for your platform from the [GroupDocs Releases website](https://releases.groupdocs.com/parser/python-net/), as the [Installation]({{< ref "parser/python-net/getting-started/installation.md" >}}) page describes. pip then finds the requirement already satisfied.

To run the examples with the full feature set, apply a license. The simplest way is the `GROUPDOCS_LIC_PATH` environment variable, which `run_all_examples.py` reads and the package applies at the first call into the engine:

{{< tabs "set-license" >}}
{{< tab "Windows" >}}
```batch
set GROUPDOCS_LIC_PATH=C:\path\to\GroupDocs.Parser.lic
```
{{< /tab >}}
{{< tab "Linux / macOS" >}}
```bash
export GROUPDOCS_LIC_PATH=/path/to/GroupDocs.Parser.lic
```
{{< /tab >}}
{{< /tabs >}}

Without a license the suite still runs, in evaluation mode: extracted text stops after about 20 lines and ends with an evaluation notice, and only the first 5 metadata items are returned (see [Licensing and evaluation]({{< ref "parser/python-net/getting-started/licensing-and-subscription.md" >}})). Each example runs in its own subprocess.

## Run the examples

Run the full suite, which prints a pass/fail summary at the end:

```bash
python run_all_examples.py
```

You can also run any single example by navigating to its folder and running the script directly. Each example uses paths relative to its own folder, and writes its results next to the script:

```bash
cd getting-started/hello-world
python hello_world.py
```

## Run with Docker

The repository ships a `Dockerfile` that builds a Linux image with Python 3.13, the system packages the library needs on Linux (ICU, fontconfig, the Liberation fonts and `libgdiplus`), and the package preinstalled. The image installs the package from PyPI, so it builds once `groupdocs-parser-net` 26.10.0 is published there.

```bash
# Build the image
docker build -t parser-examples .

# Run unlicensed (evaluation mode)
docker run --rm parser-examples

# Run with a license mounted from the host
docker run --rm \
    -v /path/to/license:/lic:ro \
    -e GROUPDOCS_LIC_PATH=/lic/your-license.lic \
    parser-examples
```

## Continuous integration

The `.github/workflows/` directory contains a workflow that runs the full example suite on `ubuntu-latest` with Python 3.13. Until `groupdocs-parser-net` 26.10.0 is on PyPI it runs on demand only (Actions > Run Examples > Run workflow); after that, on every push. The same run is reproducible locally via the `Dockerfile` above.

## Troubleshooting

- **"Couldn't find a valid ICU package"**, with the Python process ending abruptly (Linux) - install ICU (`libicu-dev` on Debian and Ubuntu). Do not set `DOTNET_SYSTEM_GLOBALIZATION_INVARIANT`.
- **"The type initializer for 'Gdip' threw an exception"** or **`DllNotFoundException: libgdiplus`** (Linux/macOS) - install `libgdiplus` (`sudo apt install libgdiplus`, or `brew install mono-libgdiplus` on macOS). Presentations need it: page previews, ODP and OTP files, and PowerPoint files with tables or hyperlinks.
- **The merged cells of a Word table come out at the wrong row** in a `get_structure()` example (Linux) - install the Liberation fonts (`sudo apt install fonts-liberation`).
- **An OCR example raises `GroupDocsParserException` naming `Microsoft.ML.OnnxRuntime.NativeMethods`** (macOS 13.0-13.2) - OCR needs macOS 13.3 or newer.
- **An `aspose_data` folder of about 80 MB appears** next to an OCR example - the first OCR call unpacks its recognition models there, with no download. Choose another folder with `Resources.storage_path` before the first OCR call.
- **`LicenseError` at the first call** - `GROUPDOCS_LIC_PATH` names a file that is missing, expired or for another product. The message gives the reason.
- **The first example takes 20-30 seconds** - that is the engine loading, once per process.
- **Evaluation marks in the output** - "Evaluation only. Created with GroupDocs.Parser for .NET." after about 20 lines of text, or an extra metadata item. Apply a license (see above); a free 30-day [temporary license](https://purchase.groupdocs.com/temporary-license) is available.

## Contribute

If you would like to add or improve an example, we encourage you to contribute to the project. All examples in this repository are open source and can be freely used in your own applications. Fork the repository, edit the source code, and open a pull request.
