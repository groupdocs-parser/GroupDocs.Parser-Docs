---
id: quick-start
url: parser/python-net/quick-start
title: Quick Start Guide
weight: 2
description: "This quick start guide shows how to extract text, images, and metadata from documents using GroupDocs.Parser for Python via .NET."
keywords: quick start, getting started, extract text, python parser
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
---

This guide demonstrates the essential steps to get started with GroupDocs.Parser for Python via .NET and perform basic document parsing operations.

## Prerequisites

Before you begin, ensure you have:

- Python 3.5 or higher installed
- GroupDocs.Parser for Python via .NET installed (see [Installation]({{< ref "parser/python-net/getting-started/installation" >}}))

## Extract Text from a Document

The most common task is extracting text from documents. Here's how to do it:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def extract_text_from_document():
    # Create an instance of Parser class
    with Parser("./sample.pdf") as parser:
        # Extract text from the document
        text_reader = parser.get_text()
        
        if text_reader is not None:
            # Print the extracted text
            print(text_reader)
        else:
            print("Text extraction isn't supported for this format")

if __name__ == "__main__":
    extract_text_from_document()
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/getting-started/quick-start/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Get Document Information

You can retrieve basic information about a document:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def get_document_information():
    # Create an instance of Parser class
    with Parser("./sample.pdf") as parser:
        # Get document info
        info = parser.get_document_info()
        
        print(f"File type: {info.file_type.file_format}")
        print(f"Page count: {info.page_count}")
        print(f"Size: {info.size} bytes")

if __name__ == "__main__":
    get_document_information()
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/getting-started/quick-start/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Extract Metadata

Extract metadata properties from documents:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def extract_metadata():
    # Create an instance of Parser class
    with Parser("./sample.pdf") as parser:
        # Extract metadata
        metadata = parser.get_metadata()
        
        if metadata is not None:
            for item in metadata:
                print(f"{item.name}: {item.value}")

if __name__ == "__main__":
    extract_metadata()
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/getting-started/quick-start/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Extract Images

Extract images from documents:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def extract_images():
    # Create an instance of Parser class
    with Parser("./sample.pdf") as parser:
        # Extract images
        images = parser.get_images()
        
        if images is not None:
            for i, image in enumerate(images):
                # Save image to file
                with open(f"image_{i}.{image.file_type.extension}", "wb") as file:
                    file.write(image.get_image_stream().read())

if __name__ == "__main__":
    extract_images()
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/getting-started/quick-start/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Extract Text from Specific Page

Extract text from a particular page:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def extract_text_from_specific_page():
    # Create an instance of Parser class
    with Parser("./sample.pdf") as parser:
        # Get document info to check page count
        info = parser.get_document_info()
        
        if info.page_count > 0:
            # Extract text from the first page (page index is 0-based)
            text_reader = parser.get_text(0)
            
            if text_reader is not None:
                print(text_reader)

if __name__ == "__main__":
    extract_text_from_specific_page()
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/getting-started/quick-start/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Check Format Support

Before processing a document, you can check if the format is supported:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def check_format_support():
    # Check if file format is supported
    if Parser.get_file_info("./sample.pdf").file_type.file_format != "Unknown":
        print("Format is supported")
        
        # Process the document
        with Parser("./sample.pdf") as parser:
            text_reader = parser.get_text()
            if text_reader is not None:
                print(text_reader)
    else:
        print("Format is not supported")

if __name__ == "__main__":
    check_format_support()
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/getting-started/quick-start/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Next Steps

Now that you've learned the basics, explore more advanced features:

- [Extract text from documents]({{< ref "parser/python-net/developer-guide/basic-usage/extract-text-from-documents" >}}) - Learn different text extraction techniques
- [Working with images]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-images" >}}) - Advanced image extraction
- [Working with tables]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-tables" >}}) - Extract tables from documents
- [Template-based parsing]({{< ref "parser/python-net/developer-guide/advanced-usage/template-based-parsing" >}}) - Parse structured data using templates

## Additional Resources

- [Supported Document Formats]({{< ref "parser/python-net/supported-document-formats" >}})
- [System Requirements]({{< ref "parser/python-net/system-requirements" >}})
- [Licensing]({{< ref "parser/python-net/licensing-and-evaluation-limitations" >}})
- [API Reference](https://reference.groupdocs.com/parser/python-net)
