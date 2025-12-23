---
id: working-with-images
url: parser/python-net/working-with-images
title: Working with Images
weight: 5
version: 25.12
description: "Enumerate, filter, and save images extracted with GroupDocs.Parser for Python via .NET."
productName: GroupDocs.Parser for Python via .NET
hideChildren: false
toc: true
tags: python, parser, images, v25.12
---

Use image areas to inspect page coordinates, image types, and to save embedded resources in the desired format.

## Save images to streams
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("brochure.pdf") as parser:
    images = parser.get_images()
    if images is None:
        print("Image extraction isn't supported for this format.")
    else:
        for image_area in images:
            with image_area.get_image_stream() as stream:
                data = stream.read()
                print(f"Read {len(data)} bytes from page {image_area.page.index + 1}")
```

{{< /tab >}}
{{< tab "brochure.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [brochure.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-images/working-with-images/brochure.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

### Tips

- `PageImageArea` exposes `rectangle`, `page`, and `file_type` so you can filter by size or format.  
- Use `save(path)` for convenience or `get_image_stream()` when you need to process bytes in memory.  
- For large documents, stop iterating once you have saved the images you need to minimize memory usage.

