---
id: extract-annotations-from-document
url: parser/python-net/extract-annotations-from-document
title: Extract annotations from document
weight: 1
description: "Learn how to extract annotations - comments, notes and their authors - from PDF, Word and PowerPoint documents using GroupDocs.Parser for Python via .NET."
keywords: extract annotations, extract PDF comments, Word comments, PowerPoint comments, annotation author, annotation timestamp, get_annotations, python
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

Documents often carry **annotations**: comments, sticky notes and other markup that reviewers add on top of the content. GroupDocs.Parser reads them with the `get_annotations` method and returns a list of `AnnotationItem` objects.

| Call | Returns |
| --- | --- |
| `get_annotations()` | The annotations of the whole document |
| `get_annotations(page_index)` | The annotations of one page (or slide); page indexes start at 0 |

Both return `None` if the document format does not support annotations, and an empty list if the document has none. Annotations are read from PDF, Word processing (DOC, DOCX, RTF) and PowerPoint (PPT, PPTX) documents; check `parser.features.annotations` when the format is not known in advance.

## Prerequisites

- GroupDocs.Parser for Python via .NET 26.10 or later installed
- A document with comments or notes

## Extract annotations from a document

Here are the steps to extract annotations from a document:

1. Instantiate the `Parser` object for the initial document;
2. Call the `get_annotations` method;
3. Iterate through the list and read each annotation's `value`.

{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./reviewed.pdf") as parser:
    # Extract annotations from the whole document
    annotations = parser.get_annotations()

    if annotations is None:
        print("Annotation extraction isn't supported for this format")
    else:
        # Print the text of each annotation
        for annotation in annotations:
            print(annotation.value)
```

{{< /tab >}}
{{< tab "reviewed.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [reviewed.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-annotations/extract-annotations-from-document/reviewed.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Prints the text of every annotation in the document, one per line: a reviewer's note and the text of a link annotation for the sample.

## Read who annotated, when and where

Each `AnnotationItem` also tells you who left the annotation, when, and on which page:

| Property | Type | Description |
| --- | --- | --- |
| `value` | `str` | The annotation's text |
| `annotation_type` | `str` | The annotation type, for example `Text` for a note or `Link` for a link |
| `page_index` | `int` | Zero-based index of the page (or slide) the annotation is on |
| `timestamp` | `datetime` or `None` | When the annotation was created or last modified, as stored in the document (no time zone) |
| `author_name` | `str` | The author's name; empty when the document does not record one |
| `author_initials` | `str` or `None` | The author's initials, when the document records them |

{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("./reviewed.pdf") as parser:
    for annotation in parser.get_annotations() or []:
        print("Type:", annotation.annotation_type)
        print("Author:", annotation.author_name or "-")
        print("Initials:", annotation.author_initials or "-")
        print("Created/modified:", annotation.timestamp)
        print("Page:", annotation.page_index + 1)
        print("Content:", annotation.value)
        print()
```

{{< /tab >}}
{{< tab "reviewed.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [reviewed.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-annotations/extract-annotations-from-document/reviewed.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** For the sample, prints:

```text
Type: Text
Author: Tomas Reyes
Initials: -
Created/modified: 2026-03-02 11:00:00
Page: 1
Content: Approve the hero crop before the print run.

Type: Link
Author: -
Initials: -
Created/modified: 2026-03-02 11:00:00
Page: 1
Content: Aurora Visuals portfolio
```

## Check that the format supports annotations

Not every format can carry annotations. Check `parser.features.annotations` first, or test the result of `get_annotations` for `None`:

{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("./sample.xlsx") as parser:
    # Check if the format supports annotations before extracting them
    if not parser.features.annotations:
        print("Annotation extraction isn't supported for this format")
    else:
        annotations = parser.get_annotations()
        print("Found %d annotations" % len(annotations))
```

{{< /tab >}}
{{< tab "sample.xlsx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.xlsx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-annotations/extract-annotations-from-document/sample.xlsx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Prints `Annotation extraction isn't supported for this format` for a spreadsheet.

## Export annotations to JSON

A review queue, an index or an AI agent usually wants annotations as data. Convert each item to a dictionary:

{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
import json

from groupdocs.parser import Parser

with Parser("./reviewed.pdf") as parser:
    records = [
        {
            "page": annotation.page_index + 1,
            "type": annotation.annotation_type,
            "author": annotation.author_name,
            "timestamp": annotation.timestamp.isoformat() if annotation.timestamp else None,
            "text": annotation.value,
        }
        for annotation in parser.get_annotations() or []
    ]

print(json.dumps(records, indent=2))
```

{{< /tab >}}
{{< tab "reviewed.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [reviewed.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-annotations/extract-annotations-from-document/reviewed.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Prints a JSON array with one object per annotation.

The [command line]({{< ref "parser/python-net/developer-guide/command-line-interface" >}}) gives the same data without code: `groupdocs-parser annotations reviewed.pdf --json`.

## Notes

- Page indexes are zero-based; add 1 when you show a page number to a person.
- `get_annotations()` returns an empty list, not `None`, for a document without annotations.
- To get the annotations' text inside the extracted text instead, see [Extract text with annotations]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-annotations/extract-text-with-annotations" >}}).

## Related pages

- [Extract annotations from document page]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-annotations/extract-annotations-from-document-page" >}})
- [Extract text with annotations]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-annotations/extract-text-with-annotations" >}})
- [Get document info]({{< ref "parser/python-net/developer-guide/basic-usage/get-document-info" >}})
