---
id: extract-metadata-from-documents
url: parser/python-net/extract-metadata-from-documents
title: Extract Metadata from Documents
weight: 7
version: 25.12
description: "Extract metadata (author, title, custom properties) from PDF, Office, images, emails, and other formats using GroupDocs.Parser for Python via .NET."
productName: GroupDocs.Parser for Python via .NET
hideChildren: false
toc: true
tags: python, parser, metadata, document-properties, v25.12
---

GroupDocs.Parser extracts metadata such as author, title, creation date, and custom properties from supported formats (see [supported formats]({{< ref "parser/python-net/supported-document-formats.md" >}})).

## Extract metadata
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("./sample.pdf") as parser:
    metadata_items = parser.get_metadata()
    if metadata_items is None:
        print("Metadata extraction is not supported for this format.")
    else:
        for item in metadata_items:
            print(f"{item.name}: {item.value}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/basic-usage/extract-metadata-from-documents/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

### Steps

1. Create a `Parser` for the target document.  
2. Call `get_metadata()` to receive a collection of metadata items.  
3. Iterate through `name` and `value` pairs and process them as needed.

For deeper parsing (attachments, text, images), combine metadata extraction with other basic usage topics.
---
id: extract-metadata-from-documents
url: parser/python-net/extract-metadata-from-documents
title: Extract Metadata from Documents
weight: 7
version: 25.12
description: "Extract metadata (author, title, custom properties) from PDF, Office, images, emails, and other formats using GroupDocs.Parser for Python via .NET."
productName: GroupDocs.Parser for Python via .NET
hideChildren: false
toc: true
tags: python, parser, metadata, document-properties, v25.12
---

GroupDocs.Parser extracts metadata such as author, title, creation date, and custom properties from supported formats (see [supported formats]({{< ref "parser/python-net/supported-document-formats.md" >}})).

## Extract metadata
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("./sample.pdf") as parser:
    metadata_items = parser.get_metadata()
    if metadata_items is None:
        print("Metadata extraction is not supported for this format.")
    else:
        for item in metadata_items:
            print(f"{item.name}: {item.value}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/basic-usage/extract-metadata-from-documents/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

### Steps

1. Create a `Parser` for the target document.  
2. Call `get_metadata()` to receive a collection of metadata items.  
3. Iterate through `name` and `value` pairs and process them as needed.

For deeper parsing (attachments, text, images), combine metadata extraction with other basic usage topics.



