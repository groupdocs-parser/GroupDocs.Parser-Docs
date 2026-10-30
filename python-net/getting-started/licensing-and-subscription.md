---
id: licensing-and-evaluation
url: parser/python-net/licensing-and-evaluation
title: Licensing and evaluation
weight: 5
keywords: free, free trial, subscription, evaluation, evaluation limitations, license, metered license, set_license, set_metered_key, GROUPDOCS_LIC_PATH, groupdocs parser python
description: "GroupDocs.Parser for Python via .NET offers flexible licensing options, including a Free Trial with evaluation limits and a 30-day Temporary License for evaluation purposes."
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
aliases:
    - /parser/python-net/licensing-and-evaluation-limitations/
    - /parser/python-net/licensing-and-subscription/
    - /parser/python-net/evaluation-limitations-and-licensing/
structuredData:
  showOrganization: True
  application:
    name: Document Parser
    description: Parse documents natively with high performance using Python language and GroupDocs.Parser for Python via .NET
    productCode: parser
    productPlatform: python-net
  howTo:
    name: How to set a license in Python
    description: Learn how to set a GroupDocs.Parser license in Python step by step
    steps:
      - name: Create an object
        text: Create an object of the License class.
      - name: Set license
        text: Call the set_license method of your object and pass the license path or a license file stream.
---

To explore the system effectively, you may want immediate access to the API. GroupDocs.Parser simplifies this by offering various purchase plans, along with a Free Trial and a 30-day Temporary License for evaluation.

{{< alert style="info" >}}

To learn more about licensing options, purchasing, and evaluation policies, refer to the [Purchase Policies and FAQ](https://purchase.groupdocs.com/policies) section.

{{< /alert >}}

## Purchased License

After purchasing GroupDocs.Parser for Python via .NET, you will receive a license file that unlocks the full functionality of the API. You can find the pricing information on the [Pricing Information](https://purchase.groupdocs.com/pricing/parser/python-net) page. A few rules apply:

- Apply the license **only once** per process, in your start-up code.
- Apply it **before** constructing any `Parser` or other GroupDocs.Parser object.
- A license can be applied from a file path, from a binary stream (handy when the license is an embedded resource), from the `GROUPDOCS_LIC_PATH` environment variable, or as a [Metered License](https://purchase.groupdocs.com/faqs/licensing/metered/) that bills by usage.

Use the [set_license](https://reference.groupdocs.com/parser/python-net/groupdocs.parser/license/) method to license the component. Calling it more than once is harmless — it simply wastes a little processor time.

### Set a license from a file

The following code snippet shows how to set a license from a file:

{{< tabs "example-1" >}}
{{< tab "set_license_from_file.py" >}}
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

### Set a license from a stream

The following code snippet shows how to set a license from a stream:

{{< tabs "example-2" >}}
{{< tab "set_license_from_stream.py" >}}
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

### Set a license from an environment variable

Set the `GROUPDOCS_LIC_PATH` environment variable to the path of a license file (or to a URL that serves one) and the license is applied automatically at the first `Parser` call. No license code is needed:

{{< tabs "set-license-env" >}}
{{< tab "Windows" >}}
```ps
$env:GROUPDOCS_LIC_PATH = "C:\path\to\GroupDocs.Parser.lic"
```
{{< /tab >}}
{{< tab "Linux / macOS" >}}
```bash
export GROUPDOCS_LIC_PATH="/path/to/GroupDocs.Parser.lic"
```
{{< /tab >}}
{{< /tabs >}}

If that license cannot be applied (an expired subscription, a license for another product, a missing file), every `Parser` call raises `LicenseError`, naming the variable and the reason, until a license is applied with `License().set_license()`. Set `GROUPDOCS_LIC_STRICT=0` to run in evaluation mode with a `LicenseWarning` instead.

### Set a metered license

A [Metered License](https://reference.groupdocs.com/parser/python-net/groupdocs.parser/metered/) is also available as an alternative to a traditional license file. It is a usage-based licensing model that may be more suitable for customers who prefer to be billed based on actual API feature usage. For more information, refer to the [Metered Licensing FAQ](https://purchase.groupdocs.com/faqs/licensing/metered).

The following sample demonstrates how to use metered licensing:

{{< tabs "example-3" >}}
{{< tab "set_metered_license.py" >}}
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

### Changing the license file name

You are not required to keep the license file name as `GroupDocs.Parser.lic`. You can rename it to any preferred name and use that name when applying the license in your application.

### "Cannot find license filename" exception

When you download the license from the GroupDocs website it is saved as `GroupDocs.Parser.lic`. However, some web browsers may automatically append `.xml`, resulting in `GroupDocs.Parser.lic.xml`.

If your Windows settings are configured to hide file extensions (the default), the file may still appear as `GroupDocs.Parser.lic` in File Explorer even though the actual name is `GroupDocs.Parser.lic.xml`. This discrepancy can cause `set_license` to throw an exception. To fix it, manually rename the file to remove the `.xml` extension, or disable "Hide extensions for known file types" in Windows.

## How to evaluate GroupDocs.Parser

You can evaluate GroupDocs.Parser for Python via .NET without purchasing a license by using one of the following options.

### Free Trial

The evaluation version is identical to the purchased one; it becomes fully licensed once you set a license. Without a license it comes with the following limitations:

| API Feature | Limit Without License |
| --- | --- |
| **Files per session** | Only 100 files |
| **Pages per document** | Only 5 pages (slides, sheets) |
| **Text extraction** | Only 20 lines per file<br/>Only the first 1600 symbols<br/>Only the first 5 pages<br/>+ Evaluation marks |
| **Metadata extraction** | Only 5 properties per file |

In evaluation mode the output itself tells you so:

- `get_text()` returns about the first 20 lines of the document, followed by the line `Evaluation only. Created with GroupDocs.Parser for .NET. Copyright 2026 Aspose Pty Ltd.`
- `get_metadata()` returns the first 5 properties, followed by an extra item whose name is that evaluation line and whose value starts with `Only the first 5 metadata items can be returned.`

If you see these marks after setting a license, the license was not applied: check the path, or set it before the first `Parser` is created.

### Temporary License

To experience the complete features of GroupDocs.Parser without the limitations of the Free Trial, you can request a 30-day ["Temporary License"](https://purchase.groupdocs.com/temporary-license).
