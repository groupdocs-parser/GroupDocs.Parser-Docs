---
id: extract-text-in-accurate-mode
url: parser/python-net/extract-text-in-accurate-mode
title: Extract text in Accurate mode
weight: 1
description: "Learn how to extract text in Accurate mode from documents using GroupDocs.Parser for Python via .NET. High-quality text extraction with best accuracy from PDF, Word, Excel and 50+ formats."
keywords: extract text, extract text in Accurate mode, accurate text extraction, high quality text extraction, PDF text extraction
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser provides the functionality to extract text from documents with the highest quality.

The **Accurate** mode is the default text extraction mode, providing the best possible text quality from documents.

You can extract text from the entire document or from individual pages.

## Prerequisites

Before you begin, ensure you have:
- GroupDocs.Parser for Python via .NET installed
- A valid license or trial
- Sample documents for testing

## Extract text from document

To extract text from the entire document in Accurate mode, use the `get_text()` method:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Extract text from the document
    text_reader = parser.get_text()
    
    # Check if text extraction is supported
    if text_reader is None:
        print("Text extraction isn't supported")
    else:
        # Print the extracted text
        print(text_reader)
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-in-accurate-mode/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** The method returns a `TextReader` object containing the entire document text, or `None` if text extraction is not supported for the document format.

## Extract text from document page

To extract text from a specific page:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Check if text extraction is supported
    if not parser.features.text:
        print("Document doesn't support text extraction")
        return
    
    # Get document info
    info = parser.get_document_info()
    
    # Check if document has pages
    if info.page_count == 0:
        print("Document has no pages")
        return
    
    # Iterate over pages
    for page_index in range(info.page_count):
        # Print page number
        print(f"
Page {page_index + 1}/{info.page_count}")
        
        # Extract text from the page
        text_reader = parser.get_text(page_index)
        
        # Print the page text
        if text_reader is not None:
            print(text_reader)
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-in-accurate-mode/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** The method extracts text from each page individually, allowing you to process documents page by page.

## Extract text with error handling

Here's a robust example with error handling:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def extract_text_safely(file_path):
    try:
        with Parser(file_path) as parser:
            # Check feature support
            if not parser.features.text:
                print(f"Text extraction not supported for {file_path}")
                return None
            
            # Extract text
            text_reader = parser.get_text()
            if text_reader is not None:
                return text_reader
            
    except Exception as e:
        print(f"Error extracting text: {e}")
        return None

# Usage
text = extract_text_safely("sample.docx")
if text:
    print(f"Extracted {len(text)} characters")
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-in-accurate-mode/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Notes

- **Accurate mode** is the default and provides the best text quality
- The `get_text()` method returns `None` if text extraction is not supported
- Use `parser.features.text` to check if text extraction is available before calling `get_text()`
- For better performance with large documents, consider extracting text page by page

## Related pages

- [Extract text in Raw mode]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-in-raw-mode" >}})
- [Search text in documents]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/search-text" >}})
- [Extract text areas]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-areas" >}})

