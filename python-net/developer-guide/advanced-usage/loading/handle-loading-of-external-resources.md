---
id: handle-loading-of-external-resources
url: parser/python-net/handle-loading-of-external-resources
title: Handle loading of external resources
weight: 5
description: "Decide which external images an HTML or MHTML document may load while GroupDocs.Parser for Python via .NET extracts its images: skip remote resources, allow a list, or audit what a document asks for."
keywords: external resources, ExternalResourceHandler, on_loading, skip remote images, HTML images, MHTML, ParserSettings, python
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

An HTML or MHTML document can reference images stored elsewhere. When GroupDocs.Parser extracts the images of such a document, it asks an **external resource handler**, if you give it one, before it loads each resource. The handler can let the resource load or skip it.

The handler receives an `ExternalResourceLoadingArgs` object:

| Member | Meaning |
| --- | --- |
| `args.uri` | The resource as the document names it, for example `installation.png` or `https://cdn.example/logo.png` |
| `args.skipped` | Set it to `True` to skip the resource |

The handler can be a function `on_loading(args)`, an object with an `on_loading(args)` method, or a subclass of `ExternalResourceHandler`. Pass it to `ParserSettings` as `external_resource_handler=`, and the settings to the `Parser` constructor.

{{< alert style="warning" >}}
Loading of external resources is enabled by default, so parsing a document from an untrusted source may make outbound HTTP requests to hosts named inside that document.

The handler is consulted only when images are extracted from HTML and MHTML documents; other extraction methods and other formats do not route through it. To guarantee that no outbound request is made, block egress at the network or process level. See [Network access and data privacy]({{< ref "parser/net/getting-started/network-access-and-data-privacy.md" >}}) for the measured per-format behaviour of the engine.
{{< /alert >}}

## Skip resources with a function

Here are the steps to handle loading of external resources:

1. Define a function that takes `args` and sets `args.skipped` for the resources to skip;
2. Create `ParserSettings` with `external_resource_handler=` your function;
3. Create the `Parser` object with the settings and call `get_images`.

{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import ParserSettings


def on_loading(args):
    # args.uri names the resource; skip every image but one
    if not args.uri.endswith("installation.png"):
        args.skipped = True


settings = ParserSettings(external_resource_handler=on_loading)

with Parser("./document.html", settings) as parser:
    images = parser.get_images()
    if images is None:
        print("Image extraction isn't supported for this format")
    else:
        print("Images:", len(images))
        for image in images:
            print(image.file_type.extension)
```

{{< /tab >}}
{{< tab "document.html" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.html](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/handle-loading-of-external-resources/document.html)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "on-loading.txt" >}}  
```text
Images: 2
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/loading/handle-loading-of-external-resources/on_loading/on-loading.txt)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** The sample references three images; with the handler only one is extracted:

```text
Images: 1
.png
```

## Audit what a document asks for

A handler that skips nothing is a simple way to see which resources a document requests. The engine may ask about the same resource more than once, so collect the URIs in a set:

{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import ParserSettings

requested = []


def on_loading(args):
    # Record every resource the document asks for, and load it
    requested.append(args.uri)


with Parser("./document.html", ParserSettings(external_resource_handler=on_loading)) as parser:
    print("Images:", len(parser.get_images() or []))

for uri in sorted(set(requested)):
    print("Requested:", uri)
```

{{< /tab >}}
{{< tab "document.html" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.html](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/handle-loading-of-external-resources/document.html)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "on-loading.txt" >}}  
```text
Images: 2
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/loading/handle-loading-of-external-resources/on_loading/on-loading.txt)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:**

```text
Images: 3
Requested: installation.png
Requested: installation_1.png
Requested: installation_2.png
```

## Subclass ExternalResourceHandler

The .NET API defines the handler as a class with an `OnLoading` method. Subclassing `ExternalResourceHandler` and overriding `on_loading` works the same way, and keeps a reusable policy in one place:

{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import ExternalResourceHandler, ParserSettings


class RemoteResourceBlocker(ExternalResourceHandler):
    """Loads local resources and skips everything on the network."""

    def on_loading(self, args):
        if args.uri.startswith(("http://", "https://", "ftp://")):
            args.skipped = True


settings = ParserSettings(external_resource_handler=RemoteResourceBlocker())

with Parser("./document.html", settings) as parser:
    print("Images:", len(parser.get_images() or []))
```

{{< /tab >}}
{{< tab "document.html" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.html](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/handle-loading-of-external-resources/document.html)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "on-loading.txt" >}}  
```text
Images: 2
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/loading/handle-loading-of-external-resources/on_loading/on-loading.txt)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** The sample's images are all local, so all three are extracted: `Images: 3`.

## Notes

- `args` is valid only during the call; do not keep it.
- A handler and a logger can be passed together, with `None` for the OCR connector: `ParserSettings(log, None, on_loading)`. See [Logging]({{< ref "parser/python-net/developer-guide/advanced-usage/logging" >}}).

## Related pages

- [Extract images from document]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-images/extract-images-from-document" >}})
- [Load options]({{< ref "parser/python-net/developer-guide/advanced-usage/loading/load-options" >}})
