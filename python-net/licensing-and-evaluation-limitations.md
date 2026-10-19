---
id: licensing-and-evaluation-limitations
url: parser/python-net/licensing-and-evaluation-limitations
title: Licensing
weight: 2
description: "Follow the instructions on this page to configure the license and find out the restrictions when using GroupDocs.Parser for Python via .NET without a license (Evaluation Limitations)"
keywords: Licensing, evaluation limitations, set metered license, setting license
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
structuredData:
  showOrganization: True
  application:
    name: Document Parser
    description: Parse documents natively with high performance using Python language and GroupDocs.Parser for Python via .NET
    productCode: parser
    productPlatform: python-net
  showVideo: True
  howTo:
    name: How to Setting License in Python
    description: Learn how to Setting License in Python step by step
    steps:
      - name: Create an object
        text: Create an object of license class.
      - name: Set license
        text: Call the set_license method of your object and put the license path or license file stream parameter.
---

Sometimes, to study the system better, you want to dive into the code as fast as possible. To make this easier, GroupDocs.Parser provides different purchase plans or offers a Free Trial and a 30-day Temporary License for evaluation.

{{< alert style="info" >}}
Note that there are several general policies and practices that guide you on how to evaluate, properly license, and purchase our products. You can find them in the ["Purchase Policies and FAQ"](https://purchase.groupdocs.com/policies) section.
{{< /alert >}}

## Free Trial or Temporary License

You can try GroupDocs.Parser without buying a license.

### Free Trial

The evaluation version is the same as the purchased one – the evaluation version simply becomes licensed when you set the license. You can set the license in several ways that are described in the next sections of this article.

The evaluation version comes with the following limitations:

- Only **100 files per session**
- Only **5 pages** (slides, sheets) of a document can be processed
- **Text extraction:** Only 20 lines per file, only the first 1600 symbols, only the first 5 pages + evaluation marks
- **Metadata extraction:** Only 5 properties per file

In evaluation mode the output itself tells you so:

- `get_text()` returns about the first 20 lines of the document, followed by the line `Evaluation only. Created with GroupDocs.Parser for .NET. Copyright 2026 Aspose Pty Ltd.`
- `get_metadata()` returns the first 5 properties, followed by an extra item whose name is that evaluation line and whose value starts with `Only the first 5 metadata items can be returned.`

If you see these marks after setting a license, the license was not applied: check the path, or set it before the first `Parser` is created.

### Temporary License

If you wish to test GroupDocs.Parser without the limitations of the trial version, you can also request a 30-day Temporary License. For details, see the ["Get a Temporary License"](https://purchase.groupdocs.com/temporary-license) page.

## How to set up a license

{{< alert style="info" >}}

You can find the pricing information on the ["Pricing Information"](https://purchase.groupdocs.com/pricing/parser/python-net) page.

{{< /alert >}}

After getting the license, you need to set it. This section describes different ways to set the license.

The license should be set:

- Only once per application domain.
- Before using any other GroupDocs.Parser classes.

{{< alert style="info" >}}

The license can be set multiple times per application domain, but we recommend doing it once since all the subsequent calls to the `set_license` method except for the first one will just waste processor time.

{{< /alert >}}

### Set License from File

The following code snippet shows how to set a license from a file:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
import os
from groupdocs.parser import License

def set_license_from_file():
    # Resolve the license path from the environment, with a local fallback
    license_path = os.environ.get("GROUPDOCS_LIC_PATH", "./GroupDocs.Parser.lic")

    # Apply the license before parsing any document
    if os.path.exists(license_path):
        License().set_license(license_path)
        print("License set successfully.")
    else:
        print("License file not found. Running in evaluation mode.")

if __name__ == "__main__":
    set_license_from_file()
```

{{< /tab >}}
{{< /tabs >}}

### Set License from Stream

The following code snippet shows how to set a license from a stream:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
import os
from groupdocs.parser import License

def set_license_from_stream():
    # Resolve the license path from the environment, with a local fallback
    license_path = os.environ.get("GROUPDOCS_LIC_PATH", "./GroupDocs.Parser.lic")

    # Apply the license from an open binary stream
    if os.path.exists(license_path):
        with open(license_path, "rb") as stream:
            License().set_license(stream)
        print("License set successfully.")
    else:
        print("License file not found. Running in evaluation mode.")

if __name__ == "__main__":
    set_license_from_stream()
```

{{< /tab >}}
{{< /tabs >}}

### Set License from an Environment Variable

Set the `GROUPDOCS_LIC_PATH` environment variable to the path of a license file (or to a URL that serves one) and the license is applied automatically when `groupdocs.parser` is imported. No license code is needed:

```bash
export GROUPDOCS_LIC_PATH="path/to/GroupDocs.Parser.lic"
```

On Windows PowerShell use `$env:GROUPDOCS_LIC_PATH = "path\to\GroupDocs.Parser.lic"`. A missing or invalid file does not raise an error; the library stays in evaluation mode, so check the output for the evaluation marks described above.

### Set Metered License

A [Metered License](https://reference.groupdocs.com/parser/python-net/groupdocs.parser/metered) is also available as an alternative to a traditional license file. It is a usage-based licensing model that may be more suitable for customers who prefer to be billed based on actual API feature usage. For more information, refer to the [Metered Licensing FAQ](https://purchase.groupdocs.com/faqs/licensing/metered).

The following sample demonstrates how to use metered licensing:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Metered

def set_metered_license():
    # Replace these placeholders with your real metered public/private keys
    public_key = "******"
    private_key = "******"

    # Skip the call until real keys are supplied (placeholder keys are rejected)
    if "*" in public_key or "*" in private_key:
        print("Provide real metered keys to enable metered licensing.")
        return

    # Instantiate Metered and set keys
    metered = Metered()
    metered.set_metered_key(public_key, private_key)

    # Get amount (MB) consumed
    consumption_quantity = Metered.get_consumption_quantity()
    print(f"Amount (MB) consumed: {consumption_quantity}")

    # Get count of credits consumed
    consumption_credit = Metered.get_consumption_credit()
    print(f"Credits consumed: {consumption_credit}")

if __name__ == "__main__":
    set_metered_license()
```

{{< /tab >}}
{{< /tabs >}}

## Evaluation Limitations Summary

| API Feature | Limit Without License |
| --- | --- |
| **Files per session** | Only 100 files |
| **Pages per document** | Only 5 pages (slides, sheets) |
| **Text extraction** | Only 20 lines per file<br/>Only the first 1600 symbols<br/>Only the first 5 pages<br/>+ Evaluation marks |
| **Metadata extraction** | Only 5 properties per file |

{{< alert style="warning" >}}
**Important:** For a full-featured evaluation without limitations, request a [temporary license](https://purchase.groupdocs.com/temporary-license) which allows you to test all features for 30 days.
{{< /alert >}}
