---
id: parse-data-from-documents
url: parser/python-net/parse-data-from-documents
title: Parse Data from Documents
weight: 4
version: 25.12
description: "Extract structured data from documents using templates with GroupDocs.Parser for Python via .NET."
productName: GroupDocs.Parser for Python via .NET
hideChildren: false
toc: true
tags: python, parser, template-parsing, data-extraction, v25.12
---

Use template-based parsing to pull structured values (like invoice numbers, dates, tables) from documents.

## Parse data with a template
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.templates import Template, TemplateField, TemplateFixedPosition
from groupdocs.parser.data import Rectangle, Point, Size

template = Template([
    TemplateField(
        TemplateFixedPosition(Rectangle(Point(35.0, 135.0), Size(100.0, 10.0))),
        "CompanyName",
    ),
    TemplateField(
        TemplateFixedPosition(Rectangle(Point(35.0, 150.0), Size(100.0, 10.0))),
        "InvoiceNumber",
    ),
])

with Parser("./invoice.pdf") as parser:
    data = parser.parse_by_template(template)
    if data is None:
        print("Parsing by template isn't supported for this format.")
    else:
        # DocumentData exposes extracted fields as an indexed collection.
        for i in range(data.count):
            field = data[i]
            area = field.page_area
            text = getattr(area, "text", None)
            print(f"{field.name}: {text if text is not None else '[non-text value]'}")
```

{{< /tab >}}
{{< tab "invoice.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [invoice.pdf](/parser/python-net/_sample_files/developer-guide/basic-usage/parse-data-from-documents/invoice.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

### Steps

1. Build a `Template` with fields or tables (fixed positions, regex, linked positions, etc.).  
2. Call `parser.parse_by_template(template)` to receive `DocumentData`.  
3. Iterate through extracted fields by index (`data.count` and `data[i]`) and process each extracted value based on its type (text, table, etc.).

For complex templates (tables, linked fields, regex fields), see the [advanced templates guide]({{< ref "parser/python-net/developer-guide/advanced-usage/template-based-parsing/_index.md" >}}).

