---
id: get-supported-file-formats
url: parser/python-net/get-supported-file-formats
title: Get supported file formats
weight: 1
description: "This article explains how to obtain supported file formats list when parsing documents with GroupDocs.Parser within your Python applications."
keywords: Get supported formats, Get file types, supported file formats
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
structuredData:
  showOrganization: True
  application:
    name: Document Parser
    description: Parse documents natively with high performance using Python language and GroupDocs.Parser for Python via .NET
    productCode: parser
    productPlatform: python-net
  showVideo: True
  howTo:
    name: Get file formats supported by Parser in Python
    description: Get file formats supported by Parser in Python step by step
    steps:
      - name: Get an array of supported file types
        text: Call the get_supported_file_types method of the FileType class. The result is a collection of FileType objects, which can be iterated.
      - name: Output the supported file types
        text: Iterate through the collection to output the supported file types, for example, to the console.
---

[GroupDocs.Parser](https://products.groupdocs.com/parser/python-net) allows you to get the list of all [supported file formats]({{< ref "parser/python-net/supported-document-formats.md" >}}). To do this, follow these steps:

1. Call the `get_supported_file_types` method of the `FileType` class
2. Enumerate through the collection of `FileType` objects

The following code snippet shows how to obtain a list of supported file formats:

## Get all supported file formats
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser.options import FileType

# Get all supported file types
supported_file_types = FileType.get_supported_file_types()

# Iterate through the collection
for file_type in sorted(supported_file_types, key=lambda x: x.extension):
    print(f"{file_type.extension} - {file_type.file_format}")

print(f"
Total supported formats: {len(list(supported_file_types))}")
```

{{< /tab >}}
{{< /tabs >}}

## Filter supported formats by category

You can filter supported formats based on your needs:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser.options import FileType

# Get all supported file types
supported_file_types = FileType.get_supported_file_types()

# Define format categories
word_formats = [".doc", ".docx", ".docm", ".dot", ".dotx", ".dotm", ".odt", ".ott", ".rtf"]
excel_formats = [".xls", ".xlsx", ".xlsm", ".xlsb", ".xlt", ".xltx", ".xltm", ".ods"]
pdf_formats = [".pdf"]

print("Word Processing Formats:")
for file_type in supported_file_types:
    if file_type.extension.lower() in word_formats:
        print(f"  {file_type.extension} - {file_type.file_format}")

print("Spreadsheet Formats:")
for file_type in supported_file_types:
    if file_type.extension.lower() in excel_formats:
        print(f"  {file_type.extension} - {file_type.file_format}")

print("PDF Formats:")
for file_type in supported_file_types:
    if file_type.extension.lower() in pdf_formats:
        print(f"  {file_type.extension} - {file_type.file_format}")
```

{{< /tab >}}
{{< /tabs >}}

## Check if a specific format is supported

You can check if a particular file format is supported:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser.options import FileType

def is_format_supported(extension):
    """Check if a file format is supported by GroupDocs.Parser"""
    supported_file_types = FileType.get_supported_file_types()
    
    for file_type in supported_file_types:
        if file_type.extension.lower() == extension.lower():
            return True
    return False

# Check various formats
formats_to_check = [".pdf", ".docx", ".xlsx", ".txt", ".unknown"]

for ext in formats_to_check:
    if is_format_supported(ext):
        print(f"{ext} - Supported")
    else:
        print(f"{ext} - Not Supported")
```

{{< /tab >}}
{{< /tabs >}}

## Get format details

You can retrieve detailed information about each supported format:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser.options import FileType

# Get all supported file types
supported_file_types = FileType.get_supported_file_types()

# Display format details
print("Supported Document Formats:\n")
print(f"{'Extension':<12} {'Format Name':<50}")
print("-" * 62)

for file_type in sorted(supported_file_types, key=lambda x: x.extension):
    print(f"{file_type.extension:<12} {file_type.file_format:<50}")
```

{{< /tab >}}
{{< /tabs >}}

## Practical usage example

Here's a practical example of validating user input:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import FileType
import os

def process_file(file_path):
    """Process a file if its format is supported"""
    
    # Get file extension
    _, ext = os.path.splitext(file_path)
    
    # Check if format is supported
    supported_file_types = FileType.get_supported_file_types()
    is_supported = any(ft.extension.lower() == ext.lower() for ft in supported_file_types)
    
    if not is_supported:
        print(f"Error: File format '{ext}' is not supported")
        return False
    
    try:
        # Process the file
        with Parser(file_path) as parser:
            doc_info = parser.get_document_info()
            print(f"Processing: {file_path}")
            print(f"Format: {doc_info.file_type.file_format}")
            print(f"Pages: {doc_info.page_count}")
            
            # Extract text
            text_reader = parser.get_text()
            if text_reader:
                print("Text extraction successful")
                return True
    except Exception as e:
        print(f"Error processing file: {e}")
        return False

# Example usage
process_file("sample.pdf")
process_file("sample.docx")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/basic-usage/get-supported-file-formats/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/basic-usage/get-supported-file-formats/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## More resources

### Advanced usage topics

To learn more about document data extraction features, please refer to the [advanced usage section]({{< ref "parser/python-net/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Parser for Python via .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET)

### Free online document parser

Along with the full-featured library, we provide a free online document parser app. You are welcome to extract data from PDF, DOCX, XLSX, and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).

