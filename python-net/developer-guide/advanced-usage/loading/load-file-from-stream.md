---
id: load-file-from-stream
url: parser/python-net/load-file-from-stream
title: Load file from stream
weight: 2
description: "This article explains how to load PDF, Word, Excel, PowerPoint documents from a stream when using GroupDocs.Parser for Python via .NET."
keywords: Load document from stream, Load document with GroupDocs.Parser
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
    name: How to load a file from a stream in Python
    description: Learn how to load a file from a stream in Python step by step
    steps:
      - name: Open file stream
        text: Open the document file as a binary stream.
      - name: Create an object and specify source file stream
        text: Create an object of Parser class. The constructor takes the source file stream.
      - name: Extract data
        text: Use parser methods to extract text, images, metadata, or other data from the document.
---

To avoid saving a file on disk, [GroupDocs.Parser](https://products.groupdocs.com/parser/python-net) allows you to work with file streams directly.

To load a document from a stream, follow these steps:

1. Open a file stream
2. Pass the opened file stream to the `Parser` class constructor

The following code snippet shows how to load a file from a stream:

## Load document from file stream
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Open the file as a binary stream
with open("sample.docx", "rb") as stream:
    # Create an instance of Parser class with the stream
    with Parser(stream) as parser:
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
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-file-from-stream/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Load document from BytesIO stream
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from io import BytesIO

# Read file into memory
with open("sample.pdf", "rb") as file:
    file_data = file.read()

# Create a BytesIO stream
stream = BytesIO(file_data)

# Create an instance of Parser class with the stream
with Parser(stream) as parser:
    # Get document info
    doc_info = parser.get_document_info()
    
    print(f"File type: {doc_info.file_type.file_format}")
    print(f"Page count: {doc_info.page_count}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-file-from-stream/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Process uploaded file without saving

This is useful for web applications where files are uploaded by users:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from io import BytesIO

def process_uploaded_file(file_content, filename):
    """Process an uploaded file without saving it to disk"""
    
    try:
        # Create a stream from the file content
        stream = BytesIO(file_content)
        
        # Create parser instance
        with Parser(stream) as parser:
            print(f"Processing uploaded file: {filename}")
            
            # Get document info
            doc_info = parser.get_document_info()
            print(f"File type: {doc_info.file_type.file_format}")
            print(f"Pages: {doc_info.page_count}")
            
            # Extract text
            text_reader = parser.get_text()
            if text_reader:
                text = text_reader
                
                return {
                    "success": True,
                    "filename": filename,
                    "type": doc_info.file_type.file_format,
                    "pages": doc_info.page_count,
                    "text_length": len(text),
                    "text": text
                }
            else:
                return {
                    "success": False,
                    "error": "Text extraction not supported"
                }
                
    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }

# Example: Simulate uploaded file
with open("sample.docx", "rb") as f:
    file_content = f.read()

result = process_uploaded_file(file_content, "sample.docx")
print(result)
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-file-from-stream/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Parser for Python via .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET)

### Free online document parser

Along with the full-featured library, we provide a free online document parser app. You are welcome to extract data from PDF, DOCX, XLSX, and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).

