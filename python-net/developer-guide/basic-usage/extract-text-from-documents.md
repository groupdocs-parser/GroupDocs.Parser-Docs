---
id: extract-text-from-documents
url: parser/python-net/extract-text-from-documents
title: Extract text from documents
weight: 3
description: "This article demonstrates how to extract text from PDF, Word, Excel, PowerPoint, Outlook, OneNote, HTML, AutoCAD, and 50+ other documents using GroupDocs.Parser for Python via .NET."
keywords: Extract text, document text extraction in Python, extract text from PDF
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
    name: How to extract text from documents in Python
    description: Learn how to extract text from documents in Python step by step
    steps:
      - name: Create an object and load the source file
        text: Create an object of Parser class. The constructor takes the source file path parameter. You may specify absolute or relative file paths as per your requirements.
      - name: Extract text
        text: Call the get_text method to extract text from the document.
      - name: Read extracted text
        text: Use the text reader object to read the extracted text.
---

Text extraction is one of the most fundamental features of [GroupDocs.Parser](https://products.groupdocs.com/parser/python-net). The API allows you to extract text from:

*   PDF documents
*   Microsoft Office documents (Word, Excel, PowerPoint)
*   Email messages
*   Images (with OCR)
*   eBooks (EPUB, FB2, CHM)
*   And 50+ other formats

GroupDocs.Parser's text extractor is easy to use and powerful at the same time. For advanced text extraction scenarios, refer to the [advanced usage section]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/_index.md" >}}).

## Extract text from documents

To extract text from documents, simply call the `get_text` method:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("./sample.docx") as parser:
  text_reader = parser.get_text()
```

{{< /tab >}}
{{< /tabs >}}

The method returns a text reader object with the extracted text.

Here are the steps to extract text from a document:

1. Instantiate the `Parser` object with the source document path
2. Call the `get_text` method and obtain the text reader object
3. Read the text from the reader

The following code snippet shows how to extract text from a document:

## Extract text from local files
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.docx") as parser:
    # Extract text into the reader
    text_reader = parser.get_text()
    
    if text_reader is not None:
        # Print the extracted text
        extracted_text = text_reader
        print(extracted_text)
    else:
        print("Text extraction isn't supported for this format")
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/basic-usage/extract-text-from-documents/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Extract text from a stream
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Open the file stream
with open("sample.pdf", "rb") as stream:
    # Create an instance of Parser class with the stream
    with Parser(stream) as parser:
        # Extract text into the reader
        text_reader = parser.get_text()
        
        if text_reader is not None:
            # Print the extracted text
            extracted_text = text_reader
            print(extracted_text)
        else:
            print("Text extraction isn't supported for this format")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/basic-usage/extract-text-from-documents/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Extract text from specific pages

You can also extract text from specific pages in a document:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("./sample.pdf") as parser:
    # Get document info to check page count
    doc_info = parser.get_document_info()
    
    # Check if the document has pages
    if doc_info.page_count > 0:
        # Extract text from the first page (page index is 0-based)
        text_reader = parser.get_text(0)
        
        if text_reader:
            print(f"Text from page 1:")
            print(text_reader)
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/basic-usage/extract-text-from-documents/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## More resources

### Advanced usage topics

To learn more about text extraction features, including formatted text, text structure, and search functionality, please refer to the [advanced usage section]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/_index.md" >}}).

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Parser for Python via .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET)

### Free online text extractor

Along with the full-featured library, we provide a free online text extractor app. You are welcome to extract text from your documents with our [Free Online Document Parser App](https://products.groupdocs.app/parser).

