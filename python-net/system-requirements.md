---
id: system-requirements
url: parser/python-net/system-requirements
title: System Requirements
linkTitle: System Requirements
weight: 4
description: "System requirements for GroupDocs.Parser for Python via .NET - supported operating systems, Python versions, wheel tags, and the Linux packages it needs."
keywords: System Requirements, Supported Operating System, Python 3.5, Python 3.14, Windows, Linux, macOS, glibc, ICU, fonts-liberation, libgdiplus
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
---

{{< alert style="info" >}}
GroupDocs.Parser for Python via .NET ships as a self-contained wheel that bundles the .NET runtime it needs. No Microsoft Office, third-party document editor, .NET install or Mono install is required. On Linux and macOS a few system packages are needed; see [Platform Dependencies](#platform-dependencies). To install the library, follow the steps in the [Installation]({{< ref "parser/python-net/getting-started/installation" >}}) guide.
{{< /alert >}}

## Supported Operating Systems

GroupDocs.Parser for Python via .NET runs on Windows, Linux, and macOS. The package ships as a platform-specific `.whl` on PyPI and `pip` picks the correct one automatically.

### Windows

* Microsoft Windows 10 (x64)
* Microsoft Windows 11 (x64)
* Microsoft Windows Server 2016 and later

The Windows wheel is 64-bit only: use a 64-bit Python. Version 25.12 also shipped a 32-bit wheel; 26.10 does not.

### Linux

* Any **x86-64** distribution with **glibc 2.27 or newer** - for example Ubuntu 18.04+, Debian 10+, RHEL 8+. The embedded .NET runtime needs glibc 2.27, and the wheel's `manylinux_2_27_x86_64` tag says so, so pip refuses an older system.

### macOS

* macOS 13 (Ventura) and later - Intel and Apple Silicon (M-series). The wheels are tagged `macosx_13_0_*`.
* **OCR needs macOS 13.3 or newer.** Recognizing text in scanned pages and images uses the bundled ONNX Runtime library, which declares macOS 13.3. On macOS 13.0-13.2 an OCR call (`TextOptions(False, True)`, or text from an image file) raises `GroupDocsParserException` naming `Microsoft.ML.OnnxRuntime.NativeMethods` and `DllNotFoundException`; every other feature works. Update macOS to use OCR.
* Microsoft supports .NET 10 on macOS 14 and later; the embedded runtime itself starts on macOS 12.

## Python Version

GroupDocs.Parser for Python via .NET supports **Python 3.5 through 3.14**. The wheel uses the `py3-none-{platform}` tag, meaning it works with any Python 3.x version in that range without per-version rebuilds.

| Python Version | Supported |
| --- | :---: |
| 3.5  | Yes |
| 3.6  | Yes |
| 3.7  | Yes |
| 3.8  | Yes |
| 3.9  | Yes |
| 3.10 | Yes |
| 3.11 | Yes |
| 3.12 | Yes |
| 3.13 | Yes |
| 3.14 | Yes |

## Package Manager

GroupDocs.Parser for Python via .NET is distributed via [PyPI](https://pypi.org/project/groupdocs-parser-net/):

```bash
pip install groupdocs-parser-net
```

The PyPI index hosts one wheel per platform:

| Platform | Wheel suffix | Oldest system |
| --- | --- | --- |
| Windows 64-bit | `py3-none-win_amd64.whl` | Windows 10, Windows Server 2016 |
| Linux x64 (glibc) | `py3-none-manylinux_2_27_x86_64.whl` | glibc 2.27 |
| macOS Apple Silicon | `py3-none-macosx_13_0_arm64.whl` | macOS 13 (13.3 for OCR) |
| macOS Intel | `py3-none-macosx_13_0_x86_64.whl` | macOS 13 (13.3 for OCR) |

`pip` 20.3 or newer picks the right wheel for your platform; older versions do not recognise these tags, so upgrade with `python -m pip install --upgrade pip` (on Python 3.5, `python -m pip install "pip==20.3.4"`). On an Intel Mac, a Python built against a pre-11 macOS SDK reports its system as macOS 10.16 - there use pip 24.1 or newer, or run `SYSTEM_VERSION_COMPAT=0 pip install groupdocs-parser-net`.

## Platform Dependencies

### Linux

Install **ICU**, **fontconfig**, the **Liberation fonts** and **libgdiplus**:

```bash
sudo apt-get update
sudo apt-get install -y libicu-dev libfontconfig1 fonts-liberation libgdiplus
```

What each package is for, measured on the 26.8 engine that 26.10 ships:

- **ICU** (`libicu-dev`, any version your distribution ships) - without it the runtime cannot start: the first call aborts the Python process with "Couldn't find a valid ICU package". Do not set `DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1` to work around it.
- **fontconfig** (`libfontconfig1`) - the bundled SkiaSharp library loads `libfontconfig.so.1`.
- **Liberation fonts** (`fonts-liberation`) - without them, `get_structure()` reports the merged cells of a Word table at the wrong row and with the wrong row span. Other fonts (DejaVu, Carlito, Caladea, or the Microsoft core fonts) do not take their place.
- **libgdiplus** (`libgdiplus`) - presentations need it. Without it, these raise `GroupDocsParserException` naming the `Gdip` type initializer and `DllNotFoundException`:
  - the page preview of every PowerPoint and OpenDocument presentation;
  - every operation except `get_metadata()` on ODP and OTP files, and on PPT, PPS and POT files that contain a table;
  - `get_hyperlinks()` on a PowerPoint file that contains a table or a hyperlink.

  Every other format works without it.

{{< alert style="info" >}}
**Not required:** the Microsoft core fonts (`ttf-mscorefonts-installer`). Every example in this documentation gives the same output with and without them.
{{< /alert >}}

### macOS

Install `mono-libgdiplus` for presentations, as on Linux:

```bash
brew install mono-libgdiplus
```

Nothing else is needed.

If you see a `DllNotFoundException: libSkiaSharp` error after upgrading, an older system copy of SkiaSharp is shadowing the one bundled with the wheel. Rename it so the bundled copy wins:

```bash
sudo mv /usr/local/lib/libSkiaSharp.dylib /usr/local/lib/libSkiaSharp.dylib.bak
```

### Windows

No extra dependencies - the wheel is self-contained.

## Development Environments

GroupDocs.Parser for Python via .NET can be used with any text editor or IDE that supports Python development, including PyCharm, Visual Studio Code, Jupyter Notebook and Spyder.
