---
id: extract-text-with-annotations
url: parser/python-net/extract-text-with-annotations
title: Extract text with annotations
weight: 3
description: "Learn how to get a document's text and its annotations in one pass with TextOptions.include_annotations in GroupDocs.Parser for Python via .NET."
keywords: extract text with annotations, include_annotations, TextOptions, comments in extracted text, python
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

Sometimes you want the document's text and its annotations in one pass, for example to index a reviewed document for search or to give an AI model the whole picture. Set `include_annotations` to `True` on `TextOptions` and pass the options to `get_text`: the annotations' text is included in the extracted text, after the text of the page each annotation is on.

## Extract text together with annotations

{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import TextOptions

with Parser("./reviewed.pdf") as parser:
    # Ask for the annotations' text together with the document text
    options = TextOptions(False)
    options.include_annotations = True

    text = parser.get_text(options)
    if text is None:
        print("Text extraction isn't supported for this format")
    else:
        print(text)
```

{{< /tab >}}
{{< tab "reviewed.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [reviewed.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-annotations/extract-text-with-annotations/reviewed.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Prints the document text. The two annotations of the sample's first page follow the text of that page.

`TextOptions(False)` keeps the default (accurate) extraction mode; `include_annotations` is `False` by default.

## Compare text with and without annotations

{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import TextOptions

with Parser("./reviewed.pdf") as parser:
    options = TextOptions(False)
    options.include_annotations = True

    plain = parser.get_text()
    with_annotations = parser.get_text(options)

    # Annotation text appears only in the second result
    for annotation in parser.get_annotations() or []:
        print("%r: %s / %s" % (annotation.value,
                               annotation.value in plain,
                               annotation.value in with_annotations))
```

{{< /tab >}}
{{< tab "reviewed.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [reviewed.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-annotations/extract-text-with-annotations/reviewed.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** For the sample, prints:

```text
'Approve the hero crop before the print run.': False / True
'Aurora Visuals portfolio': False / True
```

The [command line]({{< ref "parser/python-net/developer-guide/command-line-interface" >}}) does the same with `groupdocs-parser text reviewed.pdf --include-annotations`.

## Related pages

- [Extract annotations from document]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-annotations/extract-annotations-from-document" >}})
- [Extract text in accurate mode]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-in-accurate-mode" >}})
