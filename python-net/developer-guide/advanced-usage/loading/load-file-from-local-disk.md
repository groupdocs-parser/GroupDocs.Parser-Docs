---
id: load-file-from-local-disk
url: parser/python-net/load-file-from-local-disk
title: Load file from local disk
weight: 1
description: "This article explains how to load PDF, Word, Excel, PowerPoint documents from local disk when using GroupDocs.Parser for Python via .NET."
keywords: Load document from local disk, Load document from file path, Load document with GroupDocs.Parser
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
    name: How to load a file from local disk in Python
    description: Learn how to load a file from a local disk in Python step by step
    steps:
      - name: Create a string variable with the path to the file
        text: Create a string variable with the path to the source file. You may specify absolute or relative file paths as per your requirements.
      - name: Create an object and load the source file
        text: Create an object of Parser class by specifying the file path in the parameter.
      - name: Extract data
        text: Use parser methods to extract text, images, metadata, or other data from the document.
---

When the document file is located on the local disk, [GroupDocs.Parser](https://products.groupdocs.com/parser/python-net) allows you to load it using the `Parser` class constructor by specifying an absolute or relative path.

The following code snippet shows how to load a document from a local disk:

## Load document by file path
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Specify the file path (absolute or relative)
file_path = "sample.docx"

# Create an instance of Parser class with the file path
with Parser(file_path) as parser:
    # Extract text from the document
    text_reader = parser.get_text()
    
    if text_reader is not None:
        # Print the extracted text
        print(text_reader)
    else:
        print("Text extraction isn't supported for this format")
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-file-from-local-disk/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Load document with absolute path
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Specify an absolute file path
file_path = "sample.pdf"

# Create an instance of Parser class
with Parser(file_path) as parser:
    # Get document info
    doc_info = parser.get_document_info()
    
    print(f"File type: {doc_info.file_type.file_format}")
    print(f"Page count: {doc_info.page_count}")
    print(f"File size: {doc_info.size} bytes")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-file-from-local-disk/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Batch processing multiple files
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import os

def process_files_from_directory(directory_path):
    """Process all supported documents in a directory"""
    
    # Get all files in the directory
    for filename in os.listdir(directory_path):
        file_path = os.path.join(directory_path, filename)
        
        # Skip if not a file
        if not os.path.isfile(file_path):
            continue
        
        try:
            print(f"
Processing: {filename}")
            
            # Create parser instance
            with Parser(file_path) as parser:
                # Get document info
                doc_info = parser.get_document_info()
                print(f"  Type: {doc_info.file_type.file_format}")
                print(f"  Pages: {doc_info.page_count}")
                
                # Extract text
                text_reader = parser.get_text()
                if text_reader:
                    text = text_reader
                    print(f"  Characters: {len(text)}")
                    
        except Exception as e:
            print(f"  Error: {e}")
```

{{< /tab >}}
{{< /tabs >}}

## Error handling

It's good practice to handle potential errors when loading files:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import os

def safe_load_document(file_path):
    """Safely load a document with error handling"""
    
    # Check if file exists
    if not os.path.exists(file_path):
        print(f"Error: File not found - {file_path}")
        return None
    
    try:
        # Create parser instance
        parser = Parser(file_path)
        print(f"Document loaded successfully: {file_path}")
        return parser
        
    except Exception as e:
        print(f"Error loading document: {e}")
        return None

# Try to load a document
parser = safe_load_document("sample.pdf")

if parser:
    try:
        # Extract data
        text_reader = parser.get_text()
        if text_reader:
            print(text_reader)

```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-file-from-local-disk/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Parser for Python via .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET)

### Free online document parser

Along with the full-featured library, we provide a free online document parser app. You are welcome to extract data from PDF, DOCX, XLSX, and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).

