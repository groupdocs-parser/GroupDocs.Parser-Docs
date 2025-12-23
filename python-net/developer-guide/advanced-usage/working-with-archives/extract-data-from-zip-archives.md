---
id: extract-data-from-zip-archives
url: parser/python-net/extract-data-from-zip-archives
title: Extract data from ZIP archives
weight: 1
description: "Learn how to extract data from ZIP archives using GroupDocs.Parser for Python via .NET. Extract files, iterate through archive contents, and parse nested archives."
keywords: extract from ZIP, ZIP archive extraction, parse ZIP files, extract ZIP contents, archive parsing
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser provides functionality to extract data from ZIP archives and other container formats, allowing you to iterate through archive contents and parse individual files.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample ZIP archives for testing
- Understanding of container/archive concepts

## Detect and iterate through archive items

To iterate through files in a ZIP archive:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./archive.zip") as parser:
    # Get container items (files in the archive)
    attachments = parser.get_container()
    
    # Check if container extraction is supported
    if attachments is None:
        print("Container extraction isn't supported")
    else:
        # Iterate over attachments
        for idx, attachment in enumerate(attachments):
            print(f"
File {idx + 1}:")
            print(f"  Name: {attachment.name}")
            print(f"  Size: {attachment.size} bytes")
            print(f"  File Path: {attachment.file_path}")
```

{{< /tab >}}
{{< tab "archive.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [archive.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/extract-data-from-zip-archives/archive.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns a collection of container items representing files in the ZIP archive, or `None` if container extraction is not supported.

## Extract text from files in ZIP archive

To extract text from specific files within an archive:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class for the archive
with Parser("./documents.zip") as parser:
    # Get container items
    attachments = parser.get_container()
    
    if attachments is None:
        print("Container extraction not supported")
    else:
        # Iterate over attachments
        for attachment in attachments:
            print(f"
Processing: {attachment.name}")
            
            try:
                # Get parser for the attachment
                with attachment.open_parser() as file_parser:
                    # Extract text from the file
                    text_reader = file_parser.get_text()
                    
                    if text_reader:
                        text = text_reader
                        print(f"  Extracted {len(text)} characters")
                        print(f"  Preview: {text[:100]}...")
                    else:
                        print(f"  Text extraction not supported for this file")
            
            except Exception as e:
                print(f"  Error: {e}")
```

{{< /tab >}}
{{< tab "documents.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/extract-data-from-zip-archives/documents.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Opens each file in the archive and extracts text content using a nested parser.

## Detect file type of container items

To identify file types within an archive:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("mixed_files.zip") as parser:
    # Get container items
    attachments = parser.get_container()
    
    if attachments:
        print("Archive Contents:\n")
        
        # Group files by type
        files_by_type = {}
        
        for attachment in attachments:
            # Get file info
            try:
                with attachment.open_parser() as file_parser:
                    info = file_parser.get_document_info()
                    file_type = info.file_type.extension if info and info.file_type else "unknown"
                    
                    if file_type not in files_by_type:
                        files_by_type[file_type] = []
                    
                    files_by_type[file_type].append(attachment.name)
            
            except:
                if "unknown" not in files_by_type:
                    files_by_type["unknown"] = []
                files_by_type["unknown"].append(attachment.name)
        
        # Print summary
        for file_type, files in sorted(files_by_type.items()):
            print(f"
{file_type.upper()} files ({len(files)}):")
            for filename in files:
                print(f"  - {filename}")
```

{{< /tab >}}
{{< tab "mixed_files.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [mixed_files.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/extract-data-from-zip-archives/mixed_files.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Categorizes archive contents by file type.

## Extract specific files from archive

To extract only specific files based on criteria:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import os

def extract_specific_files(archive_path, output_dir, extensions=None):
    """
    Extract specific files from ZIP archive based on extension.
    
    Args:
        archive_path: Path to ZIP archive
        output_dir: Directory to save extracted files
        extensions: List of extensions to extract (e.g., ['.pdf', '.docx'])
    """
    os.makedirs(output_dir, exist_ok=True)
    
    with Parser(archive_path) as parser:
        attachments = parser.get_container()
        
        if not attachments:
            print("No files found in archive")
            return
        
        extracted_count = 0
        
        for attachment in attachments:
            # Check extension filter
            if extensions:
                file_ext = os.path.splitext(attachment.name)[1].lower()
                if file_ext not in extensions:
                    continue
            
            print(f"Extracting: {attachment.name}")
            
            try:
                # Open parser for the file
                with attachment.open_parser() as file_parser:
                    # Extract text
                    text_reader = file_parser.get_text()
                    
                    if text_reader:
                        # Save to output directory
                        output_file = os.path.join(output_dir, f"{attachment.name}.txt")
                        
                        with open(output_file, 'w', encoding='utf-8') as f:
                            f.write(text_reader)
                        
                        extracted_count += 1
                        print(f"  Saved to: {output_file}")
            
            except Exception as e:
                print(f"  Error: {e}")
        
        print(f"
Extracted {extracted_count} files")

# Usage - extract only PDF and DOCX files
extract_specific_files("documents.zip", "extracted", ['.pdf', '.docx'])
```

{{< /tab >}}
{{< tab "documents.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/extract-data-from-zip-archives/documents.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts and processes only files matching specified extensions.

## Process nested archives

To handle ZIP files within ZIP files:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def process_archive_recursively(parser, level=0):
    """
    Recursively process archives and nested archives.
    """
    indent = "  " * level
    
    # Get container items
    attachments = parser.get_container()
    
    if not attachments:
        return
    
    for attachment in attachments:
        print(f"{indent} {attachment.name} ({attachment.size} bytes)")
        
        try:
            # Open parser for the attachment
            with attachment.open_parser() as file_parser:
                # Check if it's also a container (nested archive)
                nested_attachments = file_parser.get_container()
                
                if nested_attachments:
                    print(f"{indent}  └─ [Archive - processing nested items]")
                    process_archive_recursively(file_parser, level + 2)
                else:
                    # Try to extract text
                    text_reader = file_parser.get_text()
                    if text_reader:
                        text = text_reader
                        print(f"{indent}  └─ Text: {len(text)} characters")
        
        except Exception as e:
            print(f"{indent}  └─ Error: {e}")

# Usage
print("Archive Structure:\n")
with Parser("nested_archive.zip") as parser:
    process_archive_recursively(parser)
```

{{< /tab >}}
{{< tab "nested_archive.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [nested_archive.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/extract-data-from-zip-archives/nested_archive.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Recursively processes nested ZIP archives, showing the complete hierarchy.

## Extract metadata from archive files

To extract metadata from files within an archive:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def extract_archive_metadata(archive_path):
    """
    Extract metadata from all files in an archive.
    """
    results = []
    
    with Parser(archive_path) as parser:
        attachments = parser.get_container()
        
        if not attachments:
            print("No files in archive")
            return results
        
        for attachment in attachments:
            file_info = {
                'name': attachment.name,
                'size': attachment.size,
                'path': attachment.file_path
            }
            
            try:
                with attachment.open_parser() as file_parser:
                    # Get metadata
                    metadata = file_parser.get_metadata()
                    
                    if metadata:
                        file_info['metadata'] = {}
                        for item in metadata:
                            file_info['metadata'][item.name] = str(item.value)
                    
                    # Get document info
                    info = file_parser.get_document_info()
                    if info:
                        file_info['file_type'] = info.file_type.extension
                        file_info['page_count'] = info.page_count
            
            except Exception as e:
                file_info['error'] = str(e)
            
            results.append(file_info)
    
    return results

# Usage
metadata_list = extract_archive_metadata("documents.zip")

print("Archive Metadata Report:\n")
for item in metadata_list:
    print(f"File: {item['name']}")
    print(f"  Size: {item['size']} bytes")
    if 'file_type' in item:
        print(f"  Type: {item['file_type']}")
    if 'page_count' in item:
        print(f"  Pages: {item['page_count']}")
    if 'metadata' in item:
        print(f"  Metadata: {len(item['metadata'])} properties")
    print()
```

{{< /tab >}}
{{< tab "documents.zip" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents.zip](/parser/python-net/_sample_files/developer-guide/advanced-usage/working-with-archives/extract-data-from-zip-archives/documents.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Generates a comprehensive metadata report for all files in the archive.

## Create archive inventory

To create a detailed inventory of archive contents:
{{< tabs "example-7" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import json

def create_archive_inventory(archive_path, output_json):
    """
    Create detailed inventory of ZIP archive contents.
    """
    inventory = {
        'archive': archive_path,
        'total_files': 0,
        'total_size': 0,
        'files': []
    }
    
    with Parser(archive_path) as parser:
        attachments = parser.get_container()
        
        if attachments:
            for attachment in attachments:
                file_entry = {
                    'name': attachment.name,
                    'size': attachment.size,
                    'path': attachment.file_path,
                    'extractable': False
                }
                
                try:
                    with attachment.open_parser() as file_parser:
                        # Check if text extraction is supported
                        if file_parser.features.text:
                            text_reader = file_parser.get_text()
                            if text_reader:
                                text = text_reader
                                file_entry['extractable'] = True
                                file_entry['text_length'] = len(text)
                                file_entry['word_count'] = len(text.split())
                except:
                    pass
                
                inventory['files'].append(file_entry)
                inventory['total_size'] += attachment.size
                inventory['total_files'] += 1
    
    # Save to JSON
    with open(output_json, 'w', encoding='utf-8') as f:
        json.dump(inventory, f, indent=2, ensure_ascii=False)
    
    print(f"Inventory created: {output_json}")
    print(f"  Total files: {inventory['total_files']}")
    print(f"  Total size: {inventory['total_size']:,} bytes")
    print(f"  Extractable: {sum(1 for f in inventory['files'] if f['extractable'])}")

# Usage
create_archive_inventory("archive.zip", "inventory.json")
```

{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Creates a JSON inventory file with detailed information about archive contents.

## Notes

- The `get_container()` method returns `None` if container extraction is not supported
- Use `open_parser()` on attachments to create a parser for individual files
- Nested archives can be processed recursively
- Each attachment has properties: `name`, `size`, and `file_path`
- The parser automatically handles nested containers (ZIP within ZIP)
- Always use context managers (`with` statements) to ensure proper resource cleanup
- Some files within archives may not support text extraction

## Related pages

- [Iterate through container items]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-archives/iterate-through-container-items" >}})
- [Detect file type of container item]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-archives/detect-file-type-of-container-item" >}})
- [Extract data from attachments]({{< ref "parser/python-net/developer-guide/basic-usage/extract-data-from-attachments-and-zip-archives" >}})

