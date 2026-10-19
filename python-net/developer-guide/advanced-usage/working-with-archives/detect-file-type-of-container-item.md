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
            print(f"\nFile: {attachment.name}")
            
            try:
                # Detect the file type of the attachment
                file_type = attachment.detect_file_type()
                
                if file_type is not None:
                    print(f"  Type: {file_type.file_format}")
                    print(f"  Extension: {file_type.extension}")
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
                detected = attachment.detect_file_type()
                
                if detected is not None:
                    file_type = detected.file_format
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
{{< tab "categorize-by-file-type.txt" >}}  
```text
Files categorized by type:

Comma Separated Values File: 1 files
  - inventory.csv

Microsoft Excel Open XML Spreadsheet: 1 files
  - inventory.xlsx

Microsoft Word Open XML Document: 1 files
  - supply-agreement-northwind.docx
[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/working-with-archives/detect-file-type-of-container-item/categorize_by_file_type/categorize-by-file-type.txt)
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
                        detected = attachment.detect_file_type()
                        file_type = detected.file_format if detected is not None else "Unknown"
                        
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

print(f"\nUnsupported files ({len(result['unsupported'])}):")
for name in result['unsupported']:
    print(f"  {name}")
```

{{< /tab >}}
{{< tab "get-supported-items.txt" >}}  
```text
Supported files (5):
  invoice-HLP-2025-0412.pdf [Portable Document Format File]
  supply-agreement-northwind.docx [Microsoft Word Open XML Document]
  inventory.xlsx [Microsoft Excel Open XML Spreadsheet]
  inventory.csv [Comma Separated Values File]
  cold-room-3.png [Portable Network Graphic]

Unsupported files (0):
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/working-with-archives/detect-file-type-of-container-item/get_supported_items/get-supported-items.txt)
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
                detected = attachment.detect_file_type()
                
                if detected is not None:
                    file_type = detected.file_format
                    extension = detected.extension
                    
                    item_info['file_type'] = file_type
                    item_info['extension'] = extension
                    
                    with attachment.open_parser() as file_parser:
                        info = file_parser.get_document_info()
                        item_info['page_count'] = info.page_count if info else None
                    
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
        print(f"\nSummary:")
        print(f"  Total items: {report['summary']['total_items']}")
        for file_type, count in sorted(type_counts.items(), key=lambda x: x[1], reverse=True):
            print(f"  {file_type}: {count}")
        
        return True

# Usage
create_file_type_report("archive.zip", "file_type_report.json")
```

{{< /tab >}}
{{< tab "file_type_report.json" >}}  
```text
{
  "container": "archive.zip",
  "items": [
    {
      "name": "invoice-HLP-2025-0412.pdf",
      "size": 212328,
      "path": "finance/invoice-HLP-2025-0412.pdf",
      "file_type": "Portable Document Format File",
      "extension": ".pdf",
      "page_count": 1
[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/working-with-archives/detect-file-type-of-container-item/create_file_type_report/file_type_report.json)
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
        expected_types: List of expected file extensions (e.g., ['.pdf', '.docx'])
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
                detected = attachment.detect_file_type()
                
                if detected is not None:
                    file_type = detected.extension
                    
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
        print(f"  Invalid items: {len(invalid_items)}\n")
        
        if invalid_items:
            print("Invalid items:")
            for item in invalid_items:
                print(f"  {item['name']}: {item['type']} - {item['reason']}")
        
        return len(invalid_items) == 0

# Usage - validate that archive contains only PDFs and Word documents
is_valid = validate_container_contents("documents.zip", ['.pdf', '.docx', '.doc'])
print(f"\nContainer is valid: {is_valid}")
```

{{< /tab >}}
{{< tab "documents.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/detect-file-type-of-container-item/documents.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "validate-container-contents.txt" >}}  
```text
Validation Results:
  Valid items: 2
  Invalid items: 3

Invalid items:
  inventory.xlsx: .xlsx - Unexpected type
  inventory.csv: .csv - Unexpected type
  cold-room-3.png: .png - Unexpected type

Container is valid: False
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/working-with-archives/detect-file-type-of-container-item/validate_container_contents/validate-container-contents.txt)
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
            print(f"\n{'='*60}")
            print(f"File: {attachment.name}")
            
            try:
                detected = attachment.detect_file_type()
                if detected is not None:
                    print(f"Type: {detected.file_format}")
                
                with attachment.open_parser() as file_parser:
                    info = file_parser.get_document_info()
                    
                    # Get metadata
                    metadata = file_parser.get_metadata()
                    
                    if metadata:
                        print("\nMetadata:")
                        for item in metadata:
                            print(f"  {item.name}: {item.value}")
                    
                    # Get page count if available
                    if info is not None:
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
{{< tab "extract-type-specific-metadata.txt" >}}  
```text
============================================================
File: invoice-HLP-2025-0412.pdf
Type: Portable Document Format File

Metadata:
  title: Invoice HLP-2025-0412
  subject: Invoice to Harbor Lane Provisions
  keywords: invoice, coffee, wholesale
  author: Alex Chen
  application: Harbor Lane Provisions
[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/working-with-archives/detect-file-type-of-container-item/extract_type_specific_metadata/extract-type-specific-metadata.txt)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts and displays type-specific metadata for each item in the container.

## Notes

- Use `detect_file_type()` on attachments to detect their file type
- The `detect_file_type()` method returns the detected `FileType` (with `file_format`, `extension`, and `format`)
- File type detection works even if the file extension is incorrect or missing
- Undetected items return a `FileType` whose `format` is `FileFormat.UNKNOWN`
- Always use try-except blocks when processing container items
- File type detection is lightweight and doesn't require full parsing

## Related pages

- [Extract data from ZIP archives]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-archives/extract-data-from-zip-archives" >}})
- [Iterate through container items]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-archives/iterate-through-container-items" >}})
- [Get document info]({{< ref "parser/python-net/developer-guide/basic-usage/get-document-info" >}})

