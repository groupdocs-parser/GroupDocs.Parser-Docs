---
id: detect-file-type-of-container-item
url: parser/python-net/detect-file-type-of-container-item
title: Detect file type of container item
weight: 3
description: "Learn how to detect file types of items within containers using GroupDocs.Parser for Python via .NET."
keywords: detect file type, file type detection, container file types, identify file format
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser provides functionality to detect the file type of items within containers (ZIP archives, OST/PST files, etc.) before processing them.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample container files
- Understanding of file type detection

## Detect file type of container items

To detect the file type of each item in a container:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./archive.zip") as parser:
    # Get container items
    attachments = parser.get_container()
    
    if attachments is None:
        print("Container extraction not supported")
    else:
        # Iterate over attachments
        for attachment in attachments:
            print(f"
File: {attachment.name}")
            
            try:
                # Open parser for the attachment
                with attachment.open_parser() as file_parser:
                    # Get document info to detect file type
                    info = file_parser.get_document_info()
                    
                    if info and info.file_type:
                        print(f"  Type: {info.file_type.file_format}")
                        print(f"  Extension: {info.file_type.extension}")
                    else:
                        print(f"  Type: Unknown")
            
            except Exception as e:
                print(f"  Error: {e}")
```

{{< /tab >}}
{{< tab "archive.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [archive.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/detect-file-type-of-container-item/archive.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Detects and displays the file type of each item in the container.

## Categorize items by file type

To organize container items by their detected file type:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from collections import defaultdict

def categorize_by_file_type(file_path):
    """
    Categorize container items by detected file type.
    """
    with Parser(file_path) as parser:
        attachments = parser.get_container()
        
        if attachments is None:
            print("Container extraction not supported")
            return {}
        
        categories = defaultdict(list)
        
        for attachment in attachments:
            try:
                with attachment.open_parser() as file_parser:
                    info = file_parser.get_document_info()
                    
                    if info and info.file_type:
                        file_type = info.file_type.file_format
                    else:
                        file_type = "Unknown"
            
            except:
                file_type = "Error"
            
            categories[file_type].append(attachment.name)
        
        return dict(categories)

# Usage
categories = categorize_by_file_type("mixed_files.zip")

print("Files categorized by type:\n")
for file_type, files in sorted(categories.items()):
    print(f"{file_type}: {len(files)} files")
    for filename in files[:3]:  # Show first 3
        print(f"  - {filename}")
    if len(files) > 3:
        print(f"  ... and {len(files) - 3} more")
    print()
```

{{< /tab >}}
{{< tab "mixed_files.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [mixed_files.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/detect-file-type-of-container-item/mixed_files.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Groups files by their detected format (PDF, DOCX, XLSX, etc.).

## Filter items by supported formats

To identify which items can be processed:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def get_supported_items(file_path):
    """
    Get list of items that support text extraction.
    """
    with Parser(file_path) as parser:
        attachments = parser.get_container()
        
        if attachments is None:
            print("Container extraction not supported")
            return []
        
        supported = []
        unsupported = []
        
        for attachment in attachments:
            try:
                with attachment.open_parser() as file_parser:
                    # Check if text extraction is supported
                    if file_parser.features.text:
                        info = file_parser.get_document_info()
                        file_type = info.file_type.file_format if info and info.file_type else "Unknown"
                        
                        supported.append({
                            'name': attachment.name,
                            'type': file_type,
                            'size': attachment.size
                        })
                    else:
                        unsupported.append(attachment.name)
            
            except:
                unsupported.append(attachment.name)
        
        return {
            'supported': supported,
            'unsupported': unsupported
        }

# Usage
result = get_supported_items("documents.zip")

print(f"Supported files ({len(result['supported'])}):")
for item in result['supported']:
    print(f"  {item['name']} [{item['type']}]")

print(f"
Unsupported files ({len(result['unsupported'])}):")
for name in result['unsupported']:
    print(f"  {name}")
```

{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Separates items into supported and unsupported categories based on feature availability.

## Create file type report

To generate a detailed report of file types in the container:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import json

def create_file_type_report(file_path, output_json):
    """
    Create detailed file type report for container contents.
    """
    with Parser(file_path) as parser:
        attachments = parser.get_container()
        
        if attachments is None:
            print("Container extraction not supported")
            return False
        
        report = {
            'container': file_path,
            'items': [],
            'summary': {}
        }
        
        type_counts = {}
        
        for attachment in attachments:
            item_info = {
                'name': attachment.name,
                'size': attachment.size,
                'path': attachment.file_path or ''
            }
            
            try:
                with attachment.open_parser() as file_parser:
                    info = file_parser.get_document_info()
                    
                    if info and info.file_type:
                        file_type = info.file_type.file_format
                        extension = info.file_type.extension
                        
                        item_info['file_type'] = file_type
                        item_info['extension'] = extension
                        item_info['page_count'] = info.page_count if hasattr(info, 'page_count') else None
                        
                        # Update counts
                        type_counts[file_type] = type_counts.get(file_type, 0) + 1
                    else:
                        item_info['file_type'] = 'Unknown'
                        type_counts['Unknown'] = type_counts.get('Unknown', 0) + 1
            
            except Exception as e:
                item_info['file_type'] = 'Error'
                item_info['error'] = str(e)
                type_counts['Error'] = type_counts.get('Error', 0) + 1
            
            report['items'].append(item_info)
        
        report['summary'] = {
            'total_items': len(report['items']),
            'type_distribution': type_counts
        }
        
        # Save report
        with open(output_json, 'w', encoding='utf-8') as f:
            json.dump(report, f, indent=2, ensure_ascii=False)
        
        print(f"File type report saved to {output_json}")
        print(f"
Summary:")
        print(f"  Total items: {report['summary']['total_items']}")
        for file_type, count in sorted(type_counts.items(), key=lambda x: x[1], reverse=True):
            print(f"  {file_type}: {count}")
        
        return True

# Usage
create_file_type_report("archive.zip", "file_type_report.json")
```

{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Creates a comprehensive JSON report with file type information and statistics.

## Detect and validate file types

To detect file types and validate against expected types:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def validate_container_contents(file_path, expected_types):
    """
    Validate that container only contains expected file types.
    
    Args:
        file_path: Path to container
        expected_types: List of expected file formats (e.g., ['Pdf', 'Docx'])
    """
    with Parser(file_path) as parser:
        attachments = parser.get_container()
        
        if attachments is None:
            print("Container extraction not supported")
            return False
        
        valid_items = []
        invalid_items = []
        
        for attachment in attachments:
            try:
                with attachment.open_parser() as file_parser:
                    info = file_parser.get_document_info()
                    
                    if info and info.file_type:
                        file_type = info.file_type.file_format
                        
                        if file_type in expected_types:
                            valid_items.append({
                                'name': attachment.name,
                                'type': file_type
                            })
                        else:
                            invalid_items.append({
                                'name': attachment.name,
                                'type': file_type,
                                'reason': 'Unexpected type'
                            })
                    else:
                        invalid_items.append({
                            'name': attachment.name,
                            'type': 'Unknown',
                            'reason': 'Type not detected'
                        })
            
            except Exception as e:
                invalid_items.append({
                    'name': attachment.name,
                    'type': 'Error',
                    'reason': str(e)
                })
        
        # Print results
        print(f"Validation Results:")
        print(f"  Valid items: {len(valid_items)}")
        print(f"  Invalid items: {len(invalid_items)}
")
        
        if invalid_items:
            print("Invalid items:")
            for item in invalid_items:
                print(f"  {item['name']}: {item['type']} - {item['reason']}")
        
        return len(invalid_items) == 0

# Usage - validate that archive contains only PDFs and Word documents
is_valid = validate_container_contents("documents.zip", ['Pdf', 'Docx', 'Doc'])
print(f"
Container is valid: {is_valid}")
```

{{< /tab >}}
{{< tab "documents.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/detect-file-type-of-container-item/documents.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Validates container contents against expected file types and reports any violations.

## Extract metadata based on file type

To extract different metadata based on detected file type:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def extract_type_specific_metadata(file_path):
    """
    Extract metadata specific to each file type.
    """
    with Parser(file_path) as parser:
        attachments = parser.get_container()
        
        if attachments is None:
            print("Container extraction not supported")
            return
        
        for attachment in attachments:
            print(f"
{'='*60}")
            print(f"File: {attachment.name}")
            
            try:
                with attachment.open_parser() as file_parser:
                    info = file_parser.get_document_info()
                    
                    if info and info.file_type:
                        print(f"Type: {info.file_type.file_format}")
                        
                        # Get metadata
                        metadata = file_parser.get_metadata()
                        
                        if metadata:
                            print("\nMetadata:")
                            for item in metadata:
                                print(f"  {item.name}: {item.value}")
                        
                        # Get page count if available
                        if hasattr(info, 'page_count'):
                            print(f"\nPages: {info.page_count}")
            
            except Exception as e:
                print(f"Error: {e}")

# Usage
extract_type_specific_metadata("mixed_documents.zip")
```

{{< /tab >}}
{{< tab "mixed_documents.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [mixed_documents.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/detect-file-type-of-container-item/mixed_documents.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts and displays type-specific metadata for each item in the container.

## Notes

- Use `open_parser()` on attachments to create a parser for file type detection
- The `get_document_info()` method provides file type information
- File type detection works even if the file extension is incorrect or missing
- Not all file types may be detected; unknown types return `None` for `file_type`
- Always use try-except blocks when processing container items
- File type detection is lightweight and doesn't require full parsing

## Related pages

- [Extract data from ZIP archives]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-archives/extract-data-from-zip-archives" >}})
- [Iterate through container items]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-archives/iterate-through-container-items" >}})
- [Get document info]({{< ref "parser/python-net/developer-guide/basic-usage/get-document-info" >}})

