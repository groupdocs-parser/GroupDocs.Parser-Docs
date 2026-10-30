---
id: extract-metadata-from-documents-advanced
url: parser/python-net/developer-guide/advanced-usage/working-with-metadata/extract-metadata-from-documents
title: Extract metadata from documents
weight: 1
description: "Learn how to extract metadata from PDF, Word, Excel, PowerPoint and 65+ document formats using GroupDocs.Parser for Python via .NET. Get document properties like author, title, creation date."
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

- **author:** Document creator
- **title:** Document title
- **subject:** Document subject or description
- **keywords:** Document keywords
- **created-time:** Creation date and time
- **last-saved-time:** Last modification date and time
- **application:** Application used to create the document
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
        
        # Extract specific fields (the names the engine reports)
        author = get_metadata_value(metadata_list, "author")
        title = get_metadata_value(metadata_list, "title")
        created = get_metadata_value(metadata_list, "created-time")
        modified = get_metadata_value(metadata_list, "last-saved-time")
        
        # Print specific metadata
        print("Document Information:")
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
{{< tab "get-metadata-value.txt" >}}  
```text
Document Information:
  Title: Supply Agreement - Northwind Coffee Roasters
  Author: Lena Park
  Created: 12/15/2025 10:00:00
  Modified: 01/12/2026 09:00:00
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata/extract-metadata-from-documents/get_metadata_value/get-metadata-value.txt)
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
{{< tab "extract-metadata-as-dict.txt" >}}  
```text
Document Metadata:
  title: Harbor Lane Provisions - Annual Report 2025
  subject: Annual report and financial summary
  keywords: annual report, wholesale, Autumn Harvest, revenue, logistics
  author: Amara Osei
  application: Aspose Pty Ltd.
  last-saved-time: 02/16/2026 12:00:00
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata/extract-metadata-from-documents/extract_metadata_as_dict/extract-metadata-as-dict.txt)
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
```text
{
  "file_path": "sample.docx",
  "properties": {
    "title": "Supply Agreement - Northwind Coffee Roasters",
    "subject": "Coffee supply agreement 2026-2028",
    "keywords": "supply agreement, coffee, Autumn Harvest, Northwind",
    "category": "Back office",
    "company": "Harbor Lane Provisions",
    "manager": "Amara Osei",
    "author": "Lena Park",
[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata/extract-metadata-from-documents/export_metadata_to_json/metadata.json)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Creates a JSON file containing all document metadata in a structured format.

## Batch metadata extraction

To extract metadata from multiple documents:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import csv

def batch_extract_metadata(file_paths, output_csv):
    """
    Extract metadata from several documents and save to CSV.
    """
    results = []
    
    for file_path in file_paths:
        print(f"Processing: {file_path}")
        
        with Parser(file_path) as parser:
            metadata = parser.get_metadata()
            
            if metadata is None:
                print("  Metadata extraction isn't supported for this format")
                continue
            
            # Extract common fields
            meta_dict = {'file_name': file_path}
            
            for item in metadata:
                meta_dict[item.name] = item.value or ""
            
            results.append(meta_dict)
    
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
        
        print(f"\nExtracted metadata from {len(results)} documents")
        print(f"Saved to {output_csv}")

# Usage
batch_extract_metadata(["sample.pdf", "sample.docx", "report.pdf"], "metadata_report.csv")
```

{{< /tab >}}
{{< tab "metadata_report.csv" >}}  
```text
application,application-version,author,category,company,created-time,file_name,keywords,last-author,last-saved-time,manager,revision-number,subject,template,title
Aspose Pty Ltd.,,Amara Osei,,,,sample.pdf,"annual report, wholesale, Autumn Harvest, revenue, logistics",,02/16/2026 12:00:00,,,Annual report and financial summary,,Harbor Lane Provisions - Annual Report 2025
Microsoft Office Word,786432,Lena Park,Back office,Harbor Lane Provisions,12/15/2025 10:00:00,sample.docx,"supply agreement, cof
[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata/extract-metadata-from-documents/batch_extract_metadata/metadata_report.csv)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Processes each listed document, extracts metadata, and generates a CSV report.

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
    
    print(f"Comparing: {file1} vs {file2}\n")
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
{{< tab "get-metadata-dict.txt" >}}  
```text
Comparing: version1.docx vs version2.docx

Field                File 1                         File 2                         Match     
-----------------------------------------------------------------------------------------------
application          Microsoft Office Word          Microsoft Office Word          Yes       
application-version  786432                         786432                         Yes       
author               Lena Park                      Lena Park                  
[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata/extract-metadata-from-documents/get_metadata_dict/get-metadata-dict.txt)
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
- [Supported document formats]({{< ref "parser/python-net/getting-started/supported-document-formats.md" >}})

