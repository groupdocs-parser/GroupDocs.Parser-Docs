---
id: working-with-templates
url: parser/python-net/working-with-templates
title: Working with Templates
weight: 3
version: 26.10
description: "Create templates with fields and tables to extract structured data using GroupDocs.Parser for Python via .NET."
productName: GroupDocs.Parser for Python via .NET
hideChildren: false
toc: true
tags: python, parser, templates, structured-data, v26.10
---

Templates let you extract structured values (fields, tables, barcodes) from recurring documents. The Python API mirrors the .NET template classes; see [`groupdocs.parser.templates`](https://reference.groupdocs.com/parser/python-net/groupdocs.parser.templates/) for the full list.

## Regex-based fields

Use `TemplateRegexPosition` to locate values by pattern and extract only the matched group:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.templates import Template, TemplateField, TemplateRegexPosition

template = Template([
    TemplateField(
        TemplateRegexPosition(r"Invoice Number\s+(?<value>[A-Z0-9\-]+)"),
        "InvoiceNumber",
    )
])

with Parser("./invoice.pdf") as parser:
    data = parser.parse_by_template(template)
    if data is not None:
        fields = data.get_fields_by_name("InvoiceNumber")
        if fields:
            invoice_number = fields[0].text
            print(f"Invoice number: {invoice_number}")
```

{{< /tab >}}
{{< tab "invoice.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [invoice.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/template-based-parsing/working-with-templates/invoice.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Tables with detector parameters

Define table bounds and column separators with `TemplateTableParameters` when you need to extract line items:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.data import Rectangle, Point, Size
from groupdocs.parser.templates import (
    Template,
    TemplateItem,
    TemplateTable,
    TemplateTableParameters,
)

table_area = Rectangle(Point(40.0, 300.0), Size(515.0, 98.0))
columns = [40.0, 120.0, 330.0, 390.0, 470.0, 555.0]

table = TemplateTable(
    parameters=TemplateTableParameters(table_area, columns),
    name="Details",
)

template = Template([table])

with Parser("./invoice.pdf") as parser:
    data = parser.parse_by_template(template)
    if data is not None:
        fields = data.get_fields_by_name("Details")
        if fields:
            details = fields[0].page_area
            print(f"Rows extracted: {details.row_count}")
```

{{< /tab >}}
{{< tab "invoice.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [invoice.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/template-based-parsing/working-with-templates/invoice.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

### Tips

- Combine regex, fixed, and linked positions in one template to anchor values reliably.  
- Keep field names unique (case-insensitive).  
- Reuse `TemplateTableLayout` when the same table structure appears on multiple pages—see the API reference for layout helpers.  
- If a template item cannot be located, the corresponding field/table is empty; handle `None` in your code.

