---
id: iterate-through-container-items
url: parser/python-net/iterate-through-container-items
title: Iterate through container items
weight: 2
description: "Learn how to iterate through container items (files in archives) using GroupDocs.Parser for Python via .NET."
keywords: iterate container items, iterate ZIP files, archive iteration, container parsing
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser provides functionality to iterate through items in container documents such as ZIP archives, OST/PST files, and documents with attachments.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample container files (ZIP, OST, PST, etc.)
- Understanding of container/attachment concepts

## Iterate through container items

To iterate through all items in a container:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./archive.zip") as parser:
    # Get container items
    attachments = parser.get_container()
    
    # Check if container extraction is supported
    if attachments is None:
        print("Container extraction isn't supported")
    else:
        # Iterate over items
        for idx, attachment in enumerate(attachments):
            print(f"
Item {idx + 1}:")
            print(f"  Name: {attachment.name}")
            print(f"  Size: {attachment.size} bytes")
            print(f"  Path: {attachment.file_path}")
```

{{< /tab >}}
{{< tab "archive.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [archive.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/iterate-through-container-items/archive.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Iterates through all files in the container and displays their properties.

## Count container items

To count files in a container:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def count_container_items(file_path):
    """
    Count the number of items in a container.
    """
    with Parser(file_path) as parser:
        attachments = parser.get_container()
        
        if attachments is None:
            return 0
        
        # Convert to list to count
        items = list(attachments)
        return len(items)

# Usage
count = count_container_items("archive.zip")
print(f"Container has {count} items")
```

{{< /tab >}}
{{< tab "archive.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [archive.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/iterate-through-container-items/archive.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns the total number of items in the container.

## List container contents with details

To display detailed information about container contents:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def list_container_contents(file_path):
    """
    List all container contents with detailed information.
    """
    with Parser(file_path) as parser:
        attachments = parser.get_container()
        
        if attachments is None:
            print("Container extraction not supported")
            return
        
        print(f"{'#':<5}{'Name':<40}{'Size':<15}{'Path':<30}")
        print("-" * 90)
        
        for idx, attachment in enumerate(attachments, 1):
            name = attachment.name[:38] if len(attachment.name) > 38 else attachment.name
            size = f"{attachment.size:,} bytes"
            path = attachment.file_path[:28] if attachment.file_path and len(attachment.file_path) > 28 else (attachment.file_path or "")
            
            print(f"{idx:<5}{name:<40}{size:<15}{path:<30}")

# Usage
list_container_contents("documents.zip")
```

{{< /tab >}}
{{< tab "documents.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/iterate-through-container-items/documents.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Displays a formatted table of all container items with their properties.

## Filter container items by extension

To filter and process only specific file types:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import os

def filter_by_extension(file_path, extensions):
    """
    Filter container items by file extension.
    
    Args:
        file_path: Path to container
        extensions: List of extensions to include (e.g., ['.pdf', '.docx'])
    """
    with Parser(file_path) as parser:
        attachments = parser.get_container()
        
        if attachments is None:
            print("Container extraction not supported")
            return []
        
        filtered_items = []
        
        for attachment in attachments:
            file_ext = os.path.splitext(attachment.name)[1].lower()
            
            if file_ext in extensions:
                filtered_items.append({
                    'name': attachment.name,
                    'size': attachment.size,
                    'extension': file_ext
                })
        
        return filtered_items

# Usage - find all PDF and Word documents
items = filter_by_extension("archive.zip", ['.pdf', '.docx', '.doc'])

print(f"Found {len(items)} PDF/Word documents:
")
for item in items:
    print(f"  {item['name']} ({item['size']:,} bytes)")
```

{{< /tab >}}
{{< tab "archive.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [archive.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/iterate-through-container-items/archive.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns only items matching the specified file extensions.

## Calculate total container size

To calculate the total size of all items:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def calculate_container_size(file_path):
    """
    Calculate total size of all items in container.
    """
    with Parser(file_path) as parser:
        attachments = parser.get_container()
        
        if attachments is None:
            print("Container extraction not supported")
            return 0
        
        total_size = 0
        item_count = 0
        
        for attachment in attachments:
            total_size += attachment.size
            item_count += 1
        
        return {
            'total_size': total_size,
            'item_count': item_count,
            'average_size': total_size / item_count if item_count > 0 else 0
        }

# Usage
stats = calculate_container_size("archive.zip")

print(f"Container Statistics:")
print(f"  Items: {stats['item_count']}")
print(f"  Total Size: {stats['total_size']:,} bytes ({stats['total_size'] / 1024 / 1024:.2f} MB)")
print(f"  Average Size: {stats['average_size']:,.0f} bytes")
```

{{< /tab >}}
{{< tab "archive.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [archive.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/iterate-through-container-items/archive.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Calculates and displays size statistics for the container.

## Group items by file type

To categorize items by file type:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from collections import defaultdict
import os

def group_by_file_type(file_path):
    """
    Group container items by file extension.
    """
    with Parser(file_path) as parser:
        attachments = parser.get_container()
        
        if attachments is None:
            print("Container extraction not supported")
            return {}
        
        groups = defaultdict(list)
        
        for attachment in attachments:
            ext = os.path.splitext(attachment.name)[1].lower() or 'no_extension'
            groups[ext].append({
                'name': attachment.name,
                'size': attachment.size
            })
        
        return dict(groups)

# Usage
groups = group_by_file_type("mixed_archive.zip")

print("Files grouped by type:\n")
for ext, items in sorted(groups.items()):
    total_size = sum(item['size'] for item in items)
    print(f"{ext.upper()} files: {len(items)} items, {total_size:,} bytes")
    for item in items[:3]:  # Show first 3
        print(f"  - {item['name']}")
    if len(items) > 3:
        print(f"  ... and {len(items) - 3} more")
    print()
```

{{< /tab >}}
{{< tab "mixed_archive.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [mixed_archive.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/iterate-through-container-items/mixed_archive.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Organizes and displays items grouped by file extension.

## Search for specific files

To search for files by name pattern:
{{< tabs "example-7" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import re

def search_container_items(file_path, pattern):
    """
    Search for items matching a name pattern.
    
    Args:
        file_path: Path to container
        pattern: Regex pattern to match file names
    """
    with Parser(file_path) as parser:
        attachments = parser.get_container()
        
        if attachments is None:
            print("Container extraction not supported")
            return []
        
        regex = re.compile(pattern, re.IGNORECASE)
        matches = []
        
        for attachment in attachments:
            if regex.search(attachment.name):
                matches.append({
                    'name': attachment.name,
                    'size': attachment.size,
                    'path': attachment.file_path
                })
        
        return matches

# Usage - find all files containing "invoice"
matches = search_container_items("documents.zip", r'invoice')

print(f"Found {len(matches)} matching files:
")
for match in matches:
    print(f"  {match['name']} ({match['size']:,} bytes)")
```

{{< /tab >}}
{{< tab "documents.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/iterate-through-container-items/documents.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns items whose names match the search pattern.

## Process items with progress tracking

To iterate with progress indication:
{{< tabs "example-8" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def process_with_progress(file_path):
    """
    Process container items with progress tracking.
    """
    with Parser(file_path) as parser:
        attachments = parser.get_container()
        
        if attachments is None:
            print("Container extraction not supported")
            return
        
        # Convert to list to get total count
        items = list(attachments)
        total = len(items)
        
        print(f"Processing {total} items...
")
        
        for idx, attachment in enumerate(items, 1):
            # Show progress
            progress = (idx / total) * 100
            print(f"[{progress:5.1f}%] Processing: {attachment.name}")
            
            # Process item (example: just count size)
            # In real scenario, you might extract text, etc.
            
            # Simulate processing
            # time.sleep(0.1)
        
        print("\nProcessing complete!")

# Usage
process_with_progress("archive.zip")
```

{{< /tab >}}
{{< tab "archive.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [archive.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/iterate-through-container-items/archive.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Processes items while showing progress percentage.

## Export container inventory to CSV

To create a CSV inventory of container contents:
{{< tabs "example-9" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import csv

def export_inventory_to_csv(file_path, output_csv):
    """
    Export container inventory to CSV file.
    """
    with Parser(file_path) as parser:
        attachments = parser.get_container()
        
        if attachments is None:
            print("Container extraction not supported")
            return False
        
        # Write to CSV
        with open(output_csv, 'w', newline='', encoding='utf-8') as csvfile:
            fieldnames = ['Index', 'Name', 'Size (bytes)', 'Path']
            writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
            
            writer.writeheader()
            
            for idx, attachment in enumerate(attachments, 1):
                writer.writerow({
                    'Index': idx,
                    'Name': attachment.name,
                    'Size (bytes)': attachment.size,
                    'Path': attachment.file_path or ''
                })
        
        print(f"Inventory exported to {output_csv}")
        return True

# Usage
export_inventory_to_csv("archive.zip", "inventory.csv")
```

{{< /tab >}}
{{< tab "archive.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [archive.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/iterate-through-container-items/archive.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "inventory.csv" >}}
{{< tab-text >}}
The following sample file is used in this example: [inventory.csv](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/iterate-through-container-items/inventory.csv)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Creates a CSV file listing all container items and their properties.

## Notes

- The `get_container()` method returns `None` if container extraction is not supported
- Container items have properties: `name`, `size`, and `file_path`
- Use `open_parser()` on items to create a parser for individual files
- Container iteration is lightweight and doesn't extract file contents
- Supports ZIP, OST, PST, and documents with attachments
- Item names may include relative paths for nested structures

## Related pages

- [Extract data from ZIP archives]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-archives/extract-data-from-zip-archives" >}})
- [Detect file type of container item]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-archives/detect-file-type-of-container-item" >}})
- [Extract data from attachments]({{< ref "parser/python-net/developer-guide/basic-usage/extract-data-from-attachments-and-zip-archives" >}})

