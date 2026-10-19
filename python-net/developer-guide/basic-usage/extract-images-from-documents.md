---
id: extract-images-from-documents
url: parser/python-net/extract-images-from-documents
title: Extract Images from Documents
weight: 8
version: 26.10
description: "Extract embedded images from PDF, Word, Excel, presentations, emails, and archives using GroupDocs.Parser for Python via .NET."
productName: GroupDocs.Parser for Python via .NET
hideChildren: false
toc: true
tags: python, parser, image-extraction, v26.10
---

GroupDocs.Parser can list and export images from supported documents (PDF, Office, emails, eBooks, and more).

## Extract images
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("./slides.pptx") as parser:
    images = parser.get_images()
    if images is None:
        print("Image extraction isn't supported for this format.")
    else:
        print(f"Found {len(images)} images")
        for index, image_area in enumerate(images, start=1):
            print(f"Page: {image_area.page.index + 1}, Type: {image_area.file_type.file_format}")
            # Save each image (the extension includes the dot, e.g. ".png")
            output_path = f"image-{index}{image_area.file_type.extension}"
            image_area.save(output_path)
```

{{< /tab >}}
{{< tab "slides.pptx" >}}
{{< tab-text >}}
The following sample file is used in this example: [slides.pptx](/parser/python-net/_sample_files/developer-guide/basic-usage/extract-images-from-documents/slides.pptx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

### Steps

1. Create `Parser` for the source file.  
2. Call `get_images()` to obtain `PageImageArea` items.  
3. Inspect `page`, `rectangle`, `file_type`, and save streams with `save()` or `get_image_stream()`.

For coordinates-based processing and format conversion, see the [advanced images guide]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-images/_index.md" >}}); the same workflow applies in Python.
