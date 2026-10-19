---
id: extract-annotations-from-document-page
url: parser/python-net/extract-annotations-from-document-page
title: Extract annotations from document page
weight: 2
description: "Learn how to extract the annotations of one page or slide, or of every page in turn, using GroupDocs.Parser for Python via .NET."
keywords: extract annotations from page, page comments, slide comments, get_annotations page_index, python
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

`get_annotations(page_index)` returns the annotations of one page (or slide) only. Page indexes start at 0.

## Extract annotations from a specific page

{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("./reviewed.pdf") as parser:
    # Extract annotations from the first page (page indexes start at 0)
    annotations = parser.get_annotations(0)

    if annotations is None:
        print("Annotation extraction isn't supported for this format")
    else:
        print("Page 1: %d annotations" % len(annotations))
        for annotation in annotations:
            print("  [%s] %s" % (annotation.annotation_type, annotation.value))
```

{{< /tab >}}
{{< tab "reviewed.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [reviewed.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-annotations/extract-annotations-from-document-page/reviewed.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** For the sample, prints:

```text
Page 1: 2 annotations
  [Text] Approve the hero crop before the print run.
  [Link] Aurora Visuals portfolio
```

## Extract annotations page by page

Use `get_document_info().page_count` to walk every page:

{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("./reviewed.pdf") as parser:
    if not parser.features.annotations:
        print("Annotation extraction isn't supported for this format")
    else:
        page_count = parser.get_document_info().page_count
        for page_index in range(page_count):
            annotations = parser.get_annotations(page_index)
            print("Page %d of %d: %d annotations" % (page_index + 1, page_count, len(annotations)))
            for annotation in annotations:
                print("  %s: %s" % (annotation.author_name or "(no author)", annotation.value))
```

{{< /tab >}}
{{< tab "reviewed.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [reviewed.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-annotations/extract-annotations-from-document-page/reviewed.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** For the two-page sample, prints:

```text
Page 1 of 2: 2 annotations
  Tomas Reyes: Approve the hero crop before the print run.
  (no author): Aurora Visuals portfolio
Page 2 of 2: 0 annotations
```

## Notes

- A page without annotations gives an empty list, not `None`.
- Every `AnnotationItem` also carries its own `page_index`, so `get_annotations()` followed by grouping on `page_index` gives the same result in one call.

## Related pages

- [Extract annotations from document]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-annotations/extract-annotations-from-document" >}})
- [Extract text with annotations]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-annotations/extract-text-with-annotations" >}})
