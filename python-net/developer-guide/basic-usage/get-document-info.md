---
id: get-document-info
url: parser/python-net/get-document-info
title: Get document info
weight: 2
description: "This article explains how to detect document file type, page count, and file size with GroupDocs.Parser for Python via .NET."
keywords: Get document info, Get file type, Page count, File size
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
    name: Get Parser document info in Python
    description: Get Parser document info in Python step by step
    steps:
      - name: Create an object and load the source file
        text: Create an object of Parser class. The constructor takes the source file path parameter. You may specify absolute or relative file paths as per your requirements.
      - name: Get document info
        text: Call the get_document_info method and assign the result to a document info object.
      - name: Access document information
        text: Use the document info object to access file type, page count, and file size properties.
---

With [GroupDocs.Parser](https://products.groupdocs.com/parser/python-net) you can retrieve the following information about a document:

*   `file_type` represents the document file type (PDF, Word document, Excel spreadsheet, PowerPoint presentation, image etc.)
*   `page_count` represents the number of pages in a document
*   `size` represents the document file size in bytes

The following code samples show how to get document information:

## Get document info from a local file
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.docx") as parser:
    # Get the document info
    doc_info = parser.get_document_info()
    
    # Print document information
    print(f"File type: {doc_info.file_type.file_format}")
    print(f"Page count: {doc_info.page_count}")
    print(f"File size: {doc_info.size} bytes")
    
    # Print file extension
    print(f"File extension: {doc_info.file_type.extension}")
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/basic-usage/get-document-info/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Get document info from a stream
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Open the file stream
with open("sample.pdf", "rb") as stream:
    # Create an instance of Parser class with the stream
    with Parser(stream) as parser:
        # Get the document info
        doc_info = parser.get_document_info()
        
        # Print document information
        print(f"File type: {doc_info.file_type.file_format}")
        print(f"Page count: {doc_info.page_count}")
        print(f"File size: {doc_info.size} bytes")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/basic-usage/get-document-info/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Check document properties before extraction

It's useful to check document properties before performing extraction operations:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def process_document(file_path):
    with Parser(file_path) as parser:
        # Get document information
        doc_info = parser.get_document_info()
        
        print(f"Processing: {file_path}")
        print(f"Type: {doc_info.file_type.file_format}")
        print(f"Pages: {doc_info.page_count}")
        print(f"Size: {doc_info.size / 1024:.2f} KB")
        
        # Process based on page count
        if doc_info.page_count > 0:
            print("Document has pages, proceeding with text extraction...")
            text_reader = parser.get_text()
            if text_reader:
                print(text_reader)
        else:
            print("Document has no pages or page count is not available")

# Process different document types
process_document("sample.pdf")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/basic-usage/get-document-info/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Get page-specific information

For multi-page documents, you can also get information about individual pages:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("./sample.docx") as parser:
    # Get document info
    doc_info = parser.get_document_info()
    
    print(f"Total pages: {doc_info.page_count}")
    
    # Iterate through pages
    for page_index in range(doc_info.page_count):
        # Extract text from each page
        print(f"
--- Page {page_index + 1} ---")
        text_reader = parser.get_text(page_index)
        if text_reader:
            page_text = text_reader
            print(f"Characters: {len(page_text)}")
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/basic-usage/get-document-info/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Working with unsupported formats

If a document format doesn't support certain features, the API returns appropriate values:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

try:
    with Parser("./unknown.format") as parser:
        doc_info = parser.get_document_info()
        
        if doc_info:
            print(f"File type: {doc_info.file_type.file_format}")
            
            # Some formats may not have page count
            if doc_info.page_count == 0:
                print("Page count is not available for this format")
        else:
            print("Could not retrieve document information")
            
except Exception as e:
    print(f"Error: {e}")
```

{{< /tab >}}
{{< /tabs >}}

## More resources

### Advanced usage topics

To learn more about document data extraction features and how to extract text, images, metadata, and more, please refer to the [advanced usage section]({{< ref "parser/python-net/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Parser for Python via .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET)

### Free online document parser

Along with the full-featured library, we provide a free online document parser app. You are welcome to extract data from PDF, DOCX, XLSX, and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).

