---
id: extract-tables-from-document
url: parser/python-net/extract-tables-from-document
title: Extract tables from document
weight: 1
description: "Learn how to extract tables from documents including Excel spreadsheets, Word documents, and PDFs using GroupDocs.Parser for Python via .NET. Complete guide with code examples for table extraction."
keywords: extract tables, extract tables from document, extract tables from Excel, Excel table extraction, extract tables from XLSX, table parser
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser provides functionality to extract tables from various document formats including **Excel spreadsheets (XLS, XLSX)**, Word documents, PDFs, and PowerPoint presentations.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample documents containing tables
- Understanding of table structure (rows, columns, cells)

## Extract tables from document

To extract all tables from a document:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.xlsx") as parser:
    # Check if table extraction is supported
    if not parser.features.tables:
        print("Table extraction isn't supported")
        return
    
    # Extract tables from the document
    tables = parser.get_tables(None)
    
    if tables:
        table_index = 0
        for table in tables:
            table_index += 1
            print(f"
=== Table {table_index} ===")
            print(f"Rows: {table.row_count}, Columns: {table.column_count}")
            
            # Iterate over rows
            for row_index in range(table.row_count):
                # Iterate over columns
                for col_index in range(table.column_count):
                    # Get table cell
                    cell = table[row_index, col_index]
                    if cell:
                        print(f"{cell.text}\t", end="")
                print()  # New line after each row
```

{{< /tab >}}
{{< tab "sample.xlsx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.xlsx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document/sample.xlsx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts all tables from the document with automatic table detection, returning structured table data with rows, columns, and cell values.

## Extract tables from Excel spreadsheets

To extract tables from Excel files:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Excel file path
excel_path = "data.xlsx"

# Create an instance of Parser class
with Parser(excel_path) as parser:
    # Check if table extraction is supported
    if not parser.features.tables:
        print("Table extraction not supported for this file")
        return
    
    # Extract tables (pass None for automatic detection)
    tables = parser.get_tables(None)
    
    if tables:
        for table_idx, table in enumerate(tables):
            print(f"
Table {table_idx + 1}:")
            print(f"Size: {table.row_count} rows × {table.column_count} columns")
            print(f"Page: {table.page.index + 1}")
            
            # Print table data
            for row in table.rows:
                row_data = []
                for cell in row.cells:
                    cell_text = cell.text if cell else ""
                    row_data.append(cell_text)
                print(" | ".join(row_data))
```

{{< /tab >}}
{{< tab "data.xlsx" >}}
{{< tab-text >}}
The following sample file is used in this example: [data.xlsx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document/data.xlsx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts structured table data from Excel spreadsheets, including all sheets and tables.

## Extract tables with custom layout

To extract tables when you know the exact table structure:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import PageTableAreaOptions
from groupdocs.parser.templates import TemplateTableLayout

# Create an instance of Parser class
with Parser("invoice.pdf") as parser:
    # Check if table extraction is supported
    if not parser.features.tables:
        print("Table extraction not supported")
        return
    
    # Define table layout (column and row separators in points)
    # Columns: x-coordinates of vertical separators
    # Rows: y-coordinates of horizontal separators
    layout = TemplateTableLayout(
        [50, 95, 275, 415, 485, 545],  # Column separators
        [325, 340, 365, 395]            # Row separators
    )
    
    # Create options for table extraction
    options = PageTableAreaOptions(layout)
    
    # Extract tables with custom layout
    tables = parser.get_tables(options)
    
    if tables:
        for table in tables:
            print(f"
Table: {table.row_count} rows × {table.column_count} columns")
            
            # Print table content
            for row in table.rows:
                for cell in row.cells:
                    if cell:
                        print(f"{cell.text} | ", end="")
                print()
```

{{< /tab >}}
{{< tab "invoice.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [invoice.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document/invoice.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts tables using predefined column and row positions, useful for structured documents like invoices or forms.

## Convert table to CSV

To export extracted tables to CSV format:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import csv
import os

def export_tables_to_csv(file_path, output_dir):
    """
    Extract tables from document and save as CSV files.
    """
    os.makedirs(output_dir, exist_ok=True)
    
    with Parser(file_path) as parser:
        if not parser.features.tables:
            print("Table extraction not supported")
            return
        
        tables = parser.get_tables(None)
        
        if not tables:
            print("No tables found")
            return
        
        for table_idx, table in enumerate(tables):
            # Create CSV filename
            csv_filename = f"table_{table_idx + 1}.csv"
            csv_path = os.path.join(output_dir, csv_filename)
            
            # Write table to CSV
            with open(csv_path, 'w', newline='', encoding='utf-8') as csvfile:
                writer = csv.writer(csvfile)
                
                # Write table rows
                for row in table.rows:
                    row_data = []
                    for cell in row.cells:
                        cell_text = cell.text if cell else ""
                        row_data.append(cell_text)
                    writer.writerow(row_data)
            
            print(f"Saved: {csv_path}")

# Usage
export_tables_to_csv("report.xlsx", "exported_tables")
```

{{< /tab >}}
{{< tab "report.xlsx" >}}
{{< tab-text >}}
The following sample file is used in this example: [report.xlsx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document/report.xlsx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Converts each extracted table to a separate CSV file preserving the table structure.

## Get table cell properties

To access detailed cell information:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("document.docx") as parser:
    if not parser.features.tables:
        print("Table extraction not supported")
        return
    
    tables = parser.get_tables(None)
    
    if tables:
        for table in tables:
            print(f"
Table: {table.row_count}×{table.column_count}")
            print(f"Position: {table.rectangle}")
            
            # Access specific cell
            if table.row_count > 0 and table.column_count > 0:
                cell = table[0, 0]  # First cell
                if cell:
                    print(f"
First cell:")
                    print(f"  Text: {cell.text}")
                    print(f"  Row index: {cell.row_index}")
                    print(f"  Column index: {cell.column_index}")
                    print(f"  Row span: {cell.row_span}")
                    print(f"  Column span: {cell.column_span}")
```

{{< /tab >}}
{{< tab "document.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document/document.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Provides access to individual cell properties including text, position, and span information.

## Extract tables from multiple pages

To extract tables with page information:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("report.pdf") as parser:
    if not parser.features.tables:
        print("Table extraction not supported")
        return
    
    # Get document info
    info = parser.get_document_info()
    print(f"Document has {info.page_count} pages")
    
    # Extract all tables
    tables = parser.get_tables(None)
    
    if tables:
        # Group tables by page
        tables_by_page = {}
        for table in tables:
            page_num = table.page.index + 1
            if page_num not in tables_by_page:
                tables_by_page[page_num] = []
            tables_by_page[page_num].append(table)
        
        # Print summary
        print(f"
Found {len(list(tables))} tables across {len(tables_by_page)} pages")
        
        for page_num in sorted(tables_by_page.keys()):
            page_tables = tables_by_page[page_num]
            print(f"
Page {page_num}: {len(page_tables)} table(s)")
            
            for idx, table in enumerate(page_tables):
                print(f"  Table {idx + 1}: {table.row_count}×{table.column_count}")
```

{{< /tab >}}
{{< tab "report.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [report.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document/report.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts all tables and organizes them by page number.

## Process large tables efficiently

To handle large tables with many rows:
{{< tabs "example-7" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def process_large_table(file_path, max_rows_to_print=10):
    """
    Process large tables efficiently, showing only a preview.
    """
    with Parser(file_path) as parser:
        if not parser.features.tables:
            print("Table extraction not supported")
            return
        
        tables = parser.get_tables(None)
        
        if tables:
            for table_idx, table in enumerate(tables):
                print(f"\nTable {table_idx + 1}:")
                print(f"Total size: {table.row_count} rows × {table.column_count} columns")
                
                # Print header (first row)
                if table.row_count > 0:
                    print("\nHeader:")
                    for col in range(table.column_count):
                        cell = table[0, col]
                        if cell:
                            print(f"  Column {col + 1}: {cell.text}")
                
                # Print preview of data
                rows_to_show = min(max_rows_to_print, table.row_count)
                print(f"
Showing first {rows_to_show} rows:")
                
                for row in range(rows_to_show):
                    row_data = []
                    for col in range(table.column_count):
                        cell = table[row, col]
                        cell_text = cell.text[:20] if cell and cell.text else ""  # Truncate long text
                        row_data.append(cell_text)
                    print(" | ".join(row_data))

# Usage
process_large_table("large_spreadsheet.xlsx")
```

{{< /tab >}}
{{< tab "large_spreadsheet.xlsx" >}}
{{< tab-text >}}
The following sample file is used in this example: [large_spreadsheet.xlsx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document/large_spreadsheet.xlsx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Notes

- Always check `parser.features.tables` before attempting table extraction
- Pass `None` to `get_tables()` for automatic table detection
- Use `TemplateTableLayout` for documents with known table structures
- Table coordinates are in document-specific units (points or pixels)
- Cells can span multiple rows or columns (check `row_span` and `column_span`)
- Empty cells return `None` when accessed via indexer
- The `get_tables()` method returns `None` if table extraction is not supported

## Related pages

- [Extract tables from document page]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-tables/extract-tables-from-document-page" >}})
- [Working with templates]({{< ref "parser/python-net/developer-guide/advanced-usage/template-based-parsing/working-with-templates" >}})
- [Get document info]({{< ref "parser/python-net/developer-guide/basic-usage/get-document-info" >}})

