---
id: extract-metadata-from-documents
url: parser/python-net/extract-metadata-from-documents
title: Extract metadata from documents
weight: 1
description: "Learn how to extract metadata from PDF, Word, Excel, PowerPoint and 50+ document formats using GroupDocs.Parser for Python via .NET. Get document properties like author, title, creation date."
keywords: Extract metadata, extract basic metadata, extract metadata from image, document properties, PDF metadata extraction, document info
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser allows extracting metadata (document properties, file information) from various document formats including PDF, Emails, Ebooks, Microsoft Office (Word, PowerPoint, Excel), LibreOffice, and many others.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample documents for testing
- Understanding of document metadata concepts

## Extract metadata from documents

To extract metadata from documents, use the `get_metadata()` method:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Extract metadata from the document
    metadata = parser.get_metadata()
    
    # Check if metadata extraction is supported
    if metadata is None:
        print("Metadata extraction isn't supported")
    else:
        # Iterate over metadata items
        for item in metadata:
            # Print metadata name and value
            print(f"{item.name}: {item.value}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata/extract-metadata-from-documents/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns a collection of `MetadataItem` objects containing document properties such as author, title, creation date, modified date, etc., or `None` if metadata extraction is not supported.

## Common metadata properties

Typical metadata properties include:

- **Author:** Document creator
- **Title:** Document title
- **Subject:** Document subject or description
- **Keywords:** Document keywords
- **CreatedTime:** Creation date and time
- **ModifiedTime:** Last modification date and time
- **Application:** Application used to create the document
- **Pages:** Number of pages (for some formats)

## Extract specific metadata fields

To extract and display specific metadata fields:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def get_metadata_value(metadata, field_name):
    """
    Get a specific metadata value by field name.
    """
    for item in metadata:
        if item.name.lower() == field_name.lower():
            return item.value
    return None

# Create an instance of Parser class
with Parser("document.docx") as parser:
    # Extract metadata
    metadata = parser.get_metadata()
    
    if metadata:
        # Convert to list for reuse
        metadata_list = list(metadata)
        
        # Extract specific fields
        author = get_metadata_value(metadata_list, "Author")
        title = get_metadata_value(metadata_list, "Title")
        created = get_metadata_value(metadata_list, "CreatedTime")
        modified = get_metadata_value(metadata_list, "ModifiedTime")
        
        # Print specific metadata
        print(f"Document Information:")
        print(f"  Title: {title}")
        print(f"  Author: {author}")
        print(f"  Created: {created}")
        print(f"  Modified: {modified}")
```

{{< /tab >}}
{{< tab "document.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata/extract-metadata-from-documents/document.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Retrieves specific metadata fields by name, making it easy to access commonly used properties.

## Export metadata to dictionary

To convert metadata to a Python dictionary:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def extract_metadata_as_dict(file_path):
    """
    Extract document metadata as a dictionary.
    """
    with Parser(file_path) as parser:
        metadata = parser.get_metadata()
        
        if metadata is None:
            return None
        
        # Convert to dictionary
        metadata_dict = {}
        for item in metadata:
            metadata_dict[item.name] = item.value
        
        return metadata_dict

# Usage
metadata = extract_metadata_as_dict("report.pdf")

if metadata:
    print("Document Metadata:")
    for key, value in metadata.items():
        print(f"  {key}: {value}")
else:
    print("Metadata extraction not supported")
```

{{< /tab >}}
{{< tab "report.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [report.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata/extract-metadata-from-documents/report.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns metadata as a dictionary with field names as keys and field values as values.

## Export metadata to JSON

To export metadata to JSON format:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import json

def export_metadata_to_json(file_path, output_file):
    """
    Extract metadata and save to JSON file.
    """
    with Parser(file_path) as parser:
        metadata = parser.get_metadata()
        
        if metadata is None:
            print("Metadata extraction not supported")
            return False
        
        # Build metadata dictionary
        metadata_dict = {
            'file_path': file_path,
            'properties': {}
        }
        
        for item in metadata:
            # Convert value to string for JSON serialization
            value = str(item.value) if item.value is not None else None
            metadata_dict['properties'][item.name] = value
        
        # Save to JSON
        with open(output_file, 'w', encoding='utf-8') as f:
            json.dump(metadata_dict, f, indent=2, ensure_ascii=False)
        
        print(f"Metadata exported to {output_file}")
        return True

# Usage
export_metadata_to_json("sample.docx", "metadata.json")
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata/extract-metadata-from-documents/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "metadata.json" >}}
{{< tab-text >}}
The following sample file is used in this example: [metadata.json](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata/extract-metadata-from-documents/metadata.json)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Creates a JSON file containing all document metadata in a structured format.

## Batch metadata extraction

To extract metadata from multiple documents:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from pathlib import Path
import csv

def batch_extract_metadata(input_dir, output_csv):
    """
    Extract metadata from all documents in a directory and save to CSV.
    """
    extensions = ['.pdf', '.docx', '.doc', '.xlsx', '.xls', '.pptx', '.ppt']
    
    results = []
    
    for file_path in Path(input_dir).rglob('*'):
        if file_path.suffix.lower() in extensions:
            print(f"Processing: {file_path.name}")
            
            try:
                with Parser(str(file_path)) as parser:
                    metadata = parser.get_metadata()
                    
                    if metadata:
                        # Extract common fields
                        meta_dict = {'file_name': file_path.name}
                        
                        for item in metadata:
                            meta_dict[item.name] = str(item.value) if item.value else ""
                        
                        results.append(meta_dict)
                    
            except Exception as e:
                print(f"  Error: {e}")
    
    # Save to CSV
    if results:
        # Get all unique field names
        all_fields = set()
        for result in results:
            all_fields.update(result.keys())
        
        # Write CSV
        with open(output_csv, 'w', newline='', encoding='utf-8') as csvfile:
            writer = csv.DictWriter(csvfile, fieldnames=sorted(all_fields))
            writer.writeheader()
            writer.writerows(results)
        
        print(f"
Extracted metadata from {len(results)} documents")
        print(f"Saved to {output_csv}")

# Usage
batch_extract_metadata("documents", "metadata_report.csv")
```

{{< /tab >}}
{{< tab "metadata_report.csv" >}}
{{< tab-text >}}
The following sample file is used in this example: [metadata_report.csv](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata/extract-metadata-from-documents/metadata_report.csv)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Processes all documents in a directory, extracts metadata, and generates a CSV report.

## Metadata comparison

To compare metadata between documents:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def compare_metadata(file1, file2):
    """
    Compare metadata between two documents.
    """
    def get_metadata_dict(file_path):
        with Parser(file_path) as parser:
            metadata = parser.get_metadata()
            if metadata:
                return {item.name: item.value for item in metadata}
            return {}
    
    metadata1 = get_metadata_dict(file1)
    metadata2 = get_metadata_dict(file2)
    
    # Find common and unique fields
    all_fields = set(metadata1.keys()) | set(metadata2.keys())
    common_fields = set(metadata1.keys()) & set(metadata2.keys())
    
    print(f"Comparing: {file1} vs {file2}
")
    print(f"{'Field':<20} {'File 1':<30} {'File 2':<30} {'Match':<10}")
    print("-" * 95)
    
    for field in sorted(all_fields):
        val1 = str(metadata1.get(field, "N/A"))[:30]
        val2 = str(metadata2.get(field, "N/A"))[:30]
        match = "Yes" if field in common_fields and metadata1.get(field) == metadata2.get(field) else "No"
        
        print(f"{field:<20} {val1:<30} {val2:<30} {match:<10}")

# Usage
compare_metadata("version1.docx", "version2.docx")
```

{{< /tab >}}
{{< tab "version1.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [version1.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata/extract-metadata-from-documents/version1.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "version2.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [version2.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata/extract-metadata-from-documents/version2.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Provides a side-by-side comparison of metadata fields from two documents.

## Notes

- The `get_metadata()` method returns `None` if metadata extraction is not supported
- Use `parser.features.metadata` to check if metadata extraction is available
- Metadata properties vary by document format
- Some formats provide more metadata than others
- Date/time values are typically returned as string or datetime objects
- Metadata extraction is lightweight and fast

## Related pages

- [Get document info]({{< ref "parser/python-net/developer-guide/basic-usage/get-document-info" >}})
- [Extract text from documents]({{< ref "parser/python-net/developer-guide/basic-usage/extract-text-from-documents" >}})
- [Supported document formats]({{< ref "parser/python-net/supported-document-formats" >}})

