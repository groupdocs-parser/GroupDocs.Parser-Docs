---
id: extract-tables-from-document-page
url: parser/python-net/extract-tables-from-document-page
title: Extract tables from document page
weight: 2
description: "Learn how to extract tables from specific document pages using GroupDocs.Parser for Python via .NET. Page-by-page table extraction from PDF, Word, Excel."
keywords: extract tables from page, page table extraction, extract tables by page, table extraction from specific page
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser allows you to extract tables from specific pages of a document, providing precise control over which tables to extract.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample documents with tables on multiple pages
- Basic understanding of page indexing (zero-based)

## Extract tables from a specific page

To extract tables from a particular page:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageTableAreaOptions

# Create an instance of Parser class
with Parser("./report.pdf") as parser:
    # Check if table extraction is supported
    if not parser.features.tables:
        print("Table extraction not supported")
        return
    
    # Create options for page-specific extraction
    page_index = 0  # First page
    options = PageTableAreaOptions(page_index)
    
    # Extract tables from the specified page
    tables = parser.get_tables(options)
    
    if tables:
        print(f"Tables on page {page_index + 1}:")
        for table_idx, table in enumerate(tables):
            print(f"
Table {table_idx + 1}:")
            print(f"Size: {table.row_count} rows × {table.column_count} columns")
            
            # Print table content
            for row in range(table.row_count):
                row_data = []
                for col in range(table.column_count):
                    cell = table[row, col]
                    cell_text = cell.text if cell else ""
                    row_data.append(cell_text)
                print(" | ".join(row_data))
    else:
        print(f"No tables found on page {page_index + 1}")
```

{{< /tab >}}
{{< tab "report.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [report.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document-page/report.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts only tables from the specified page, returning an empty collection if no tables exist on that page.

## Extract tables from all pages iteratively

To process tables page by page:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageTableAreaOptions

# Create an instance of Parser class
with Parser("./document.docx") as parser:
    # Check if table extraction is supported
    if not parser.features.tables:
        print("Table extraction not supported")
        return
    
    # Get document info
    info = parser.get_document_info()
    
    if info.page_count == 0:
        print("Document has no pages")
        return
    
    # Iterate over pages
    total_tables = 0
    for page_index in range(info.page_count):
        print(f"
{'=' * 50}")
        print(f"Page {page_index + 1}/{info.page_count}")
        print('=' * 50)
        
        # Create options for this page
        options = PageTableAreaOptions(page_index)
        
        # Extract tables from current page
        tables = parser.get_tables(options)
        
        if tables:
            page_table_count = 0
            for table in tables:
                page_table_count += 1
                total_tables += 1
                
                print(f"
Table {page_table_count}:")
                print(f"Size: {table.row_count}×{table.column_count}")
                print(f"Position: ({table.rectangle.left}, {table.rectangle.top})")
            
            print(f"
Tables on this page: {page_table_count}")
        else:
            print("No tables on this page")
    
    print(f"
{'=' * 50}")
    print(f"Total tables in document: {total_tables}")
```

{{< /tab >}}
{{< tab "document.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document-page/document.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Iterates through all pages and extracts tables from each page individually, providing page-specific statistics.

## Extract tables from specific pages only

To extract tables from selected pages:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageTableAreaOptions

# Create an instance of Parser class
with Parser("report.xlsx") as parser:
    if not parser.features.tables:
        print("Table extraction not supported")
        return
    
    # Define pages to process (e.g., pages 1, 3, and 5)
    pages_to_process = [0, 2, 4]  # Zero-based indices
    
    for page_index in pages_to_process:
        print(f"
Processing page {page_index + 1}...")
        
        # Create options for this page
        options = PageTableAreaOptions(page_index)
        
        # Extract tables from this page
        tables = parser.get_tables(options)
        
        if tables:
            for table_idx, table in enumerate(tables):
                print(f"  Table {table_idx + 1}: {table.row_count}×{table.column_count}")
                
                # Show first row as sample
                if table.row_count > 0:
                    print("  First row:", end=" ")
                    for col in range(min(3, table.column_count)):  # Show first 3 columns
                        cell = table[0, col]
                        if cell:
                            print(f"[{cell.text}]", end=" ")
                    print("...")
        else:
            print(f"  No tables found on page {page_index + 1}")
```

{{< /tab >}}
{{< tab "report.xlsx" >}}
{{< tab-text >}}
The following sample file is used in this example: [report.xlsx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document-page/report.xlsx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts tables only from specified pages, skipping others for efficiency.

## Compare tables across pages

To analyze table structure across different pages:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageTableAreaOptions

def compare_tables_across_pages(file_path, page_indices):
    """
    Compare table structures across multiple pages.
    """
    with Parser(file_path) as parser:
        if not parser.features.tables:
            print("Table extraction not supported")
            return
        
        page_table_info = {}
        
        for page_index in page_indices:
            options = PageTableAreaOptions(page_index)
            tables = parser.get_tables(options)
            
            if tables:
                table_list = list(tables)
                page_table_info[page_index + 1] = {
                    'count': len(table_list),
                    'structures': [(t.row_count, t.column_count) for t in table_list]
                }
            else:
                page_table_info[page_index + 1] = {
                    'count': 0,
                    'structures': []
                }
        
        # Print comparison
        print("Table Structure Comparison:")
        print(f"{'Page':<10}{'Tables':<10}{'Structures':<40}")
        print("-" * 60)
        
        for page_num, info in sorted(page_table_info.items()):
            structures_str = ", ".join([f"{r}×{c}" for r, c in info['structures']])
            print(f"{page_num:<10}{info['count']:<10}{structures_str:<40}")

# Usage
compare_tables_across_pages("financial_report.pdf", [0, 1, 2, 3, 4])
```

{{< /tab >}}
{{< tab "financial_report.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [financial_report.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document-page/financial_report.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Provides a comparative view of table structures across specified pages.

## Export tables from specific page to JSON

To export page-specific tables to structured JSON:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageTableAreaOptions
import json

def export_page_tables_to_json(file_path, page_index, output_file):
    """
    Export tables from a specific page to JSON format.
    """
    with Parser(file_path) as parser:
        if not parser.features.tables:
            print("Table extraction not supported")
            return
        
        options = PageTableAreaOptions(page_index)
        tables = parser.get_tables(options)
        
        if not tables:
            print(f"No tables found on page {page_index + 1}")
            return
        
        # Build JSON structure
        page_data = {
            'page_number': page_index + 1,
            'tables': []
        }
        
        for table_idx, table in enumerate(tables):
            table_data = {
                'table_index': table_idx + 1,
                'rows': table.row_count,
                'columns': table.column_count,
                'data': []
            }
            
            # Extract table data
            for row in range(table.row_count):
                row_data = []
                for col in range(table.column_count):
                    cell = table[row, col]
                    row_data.append(cell.text if cell else "")
                table_data['data'].append(row_data)
            
            page_data['tables'].append(table_data)
        
        # Save to JSON
        with open(output_file, 'w', encoding='utf-8') as f:
            json.dump(page_data, f, indent=2, ensure_ascii=False)
        
        print(f"Exported {len(page_data['tables'])} tables from page {page_index + 1} to {output_file}")

# Usage
export_page_tables_to_json("data.pdf", 0, "page1_tables.json")
```

{{< /tab >}}
{{< tab "data.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [data.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document-page/data.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "page1_tables.json" >}}
{{< tab-text >}}
The following sample file is used in this example: [page1_tables.json](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document-page/page1_tables.json)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Creates a JSON file containing all tables from the specified page with complete data.

## Extract tables with custom layout from specific page

To use custom table layout for a specific page:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageTableAreaOptions
from groupdocs.parser.templates import TemplateTableLayout

# Create an instance of Parser class
with Parser("invoice.pdf") as parser:
    if not parser.features.tables:
        print("Table extraction not supported")
        return
    
    # Define custom table layout
    layout = TemplateTableLayout(
        [50, 150, 300, 450],  # Column separators
        [200, 220, 240, 260]  # Row separators
    )
    
    # Create options for specific page with custom layout
    page_index = 0
    options = PageTableAreaOptions(page_index, layout)
    
    # Extract tables
    tables = parser.get_tables(options)
    
    if tables:
        print(f"Tables extracted from page {page_index + 1} with custom layout:")
        for table in tables:
            print(f"
Table: {table.row_count}×{table.column_count}")
            for row in range(table.row_count):
                for col in range(table.column_count):
                    cell = table[row, col]
                    if cell:
                        print(f"{cell.text:20}", end=" | ")
                print()
```

{{< /tab >}}
{{< tab "invoice.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [invoice.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document-page/invoice.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts tables from the specified page using the custom layout definition.

## Notes

- Page indices are zero-based (first page is index 0)
- Use `PageTableAreaOptions(page_index)` to extract from a specific page
- Combine with `TemplateTableLayout` for structured documents with known table positions
- Use `get_document_info()` to determine the total number of pages
- Page-by-page extraction is more memory-efficient for large documents
- Empty collections are returned for pages without tables (not `None`)

## Related pages

- [Extract tables from document]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document" >}})
- [Working with templates]({{< ref "parser/python-net/developer-guide/advanced-usage/template-based-parsing/working-with-templates" >}})
- [Get document info]({{< ref "parser/python-net/developer-guide/basic-usage/get-document-info" >}})

