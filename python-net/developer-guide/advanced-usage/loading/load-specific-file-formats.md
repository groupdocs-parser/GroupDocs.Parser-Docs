---
id: load-specific-file-formats
url: parser/python-net/load-specific-file-formats
title: Load specific file formats
weight: 4
description: "Learn how to load specific file formats manually using LoadOptions in GroupDocs.Parser for Python via .NET."
keywords: Load specific file formats, LoadOptions, FileFormat, manual format specification
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
---

In some cases, it's required to specify the document format manually to guarantee correct output produced by GroupDocs.Parser. The following are the cases when the document format must be specified manually:

*   Markdown documents
*   MHTML documents
*   OTP documents (OpenDocument Presentation Template)
*   Databases
*   Emails from remote servers

Here are the steps to specify the document format:

1. Instantiate the `LoadOptions` object and pass the document format in the constructor
2. Create `Parser` object with the file path and `LoadOptions`
3. Call extraction methods as usual

## Load Markdown document

The following example shows how to specify the document format for Markdown document:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import LoadOptions, FileType

# Create LoadOptions for Markdown format
load_options = LoadOptions(FileType.MD)

# Open the file stream
with open("sample.md", "rb") as stream:
    # Create an instance of Parser class with LoadOptions
    with Parser(stream, load_options) as parser:
        # Extract text from the Markdown document
        text_reader = parser.get_text()
        
        if text_reader is not None:
            # Print the extracted text
            # Markdown is detected; text without special symbols is printed
            print(text_reader.read_to_end())
        else:
            print("Text extraction isn't supported")
```

{{< /tab >}}
{{< tab "sample.md" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.md](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-specific-file-formats/sample.md)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Load MHTML document
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import LoadOptions, FileType

# Create LoadOptions for MHTML format
load_options = LoadOptions(FileType.MHTML)

# Load MHTML document
with Parser("email_page.mhtml", load_options) as parser:
    # Extract text
    text_reader = parser.get_text()
    
    if text_reader is not None:
        print(text_reader.read_to_end())
```

{{< /tab >}}
{{< tab "email_page.mhtml" >}}
{{< tab-text >}}
The following sample file is used in this example: [email_page.mhtml](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-specific-file-formats/email_page.mhtml)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Load OTP document
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import LoadOptions, FileType

# Create LoadOptions for OTP (OpenDocument Presentation Template) format
load_options = LoadOptions(FileType.OTP)

# Load OTP document
with Parser("template.otp", load_options) as parser:
    # Get document info
    doc_info = parser.get_document_info()
    
    print(f"File type: {doc_info.file_type.file_format}")
    print(f"Pages: {doc_info.page_count}")
    
    # Extract text
    text_reader = parser.get_text()
    if text_reader is not None:
        print(text_reader.read_to_end())
```

{{< /tab >}}
{{< tab "template.otp" >}}
{{< tab-text >}}
The following sample file is used in this example: [template.otp](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-specific-file-formats/template.otp)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Detect format and load appropriately

You can create a helper function to detect and load documents with appropriate format:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import LoadOptions, FileType
import os

def get_file_format(file_path):
    """Detect file format based on extension"""
    
    _, ext = os.path.splitext(file_path)
    ext = ext.lower()
    
    format_map = {
        '.md': FileType.MD,
        '.markdown': FileType.MD,
        '.mhtml': FileType.MHTML,
        '.mht': FileType.MHTML,
        '.otp': FileType.OTP,
    }
    
    return format_map.get(ext)

def load_document_with_format(file_path):
    """Load document with appropriate format specification"""
    
    file_format = get_file_format(file_path)
    
    if file_format:
        print(f"Loading with explicit format: {file_format}")
        load_options = LoadOptions(file_format)
        parser = Parser(file_path, load_options)
    else:
        print("Loading with auto-detection")
        parser = Parser(file_path)
    
    return parser

# Usage examples
with load_document_with_format("webpage.mhtml") as parser:
    text_reader = parser.get_text()
    if text_reader is not None:
        print(text_reader.read_to_end())
```

{{< /tab >}}
{{< tab "webpage.mhtml" >}}
{{< tab-text >}}
The following sample file is used in this example: [webpage.mhtml](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-specific-file-formats/webpage.mhtml)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "load-document-with-format.txt" >}}  
```text
Loading with explicit format: MIME HTML File(.mhtml)
Subject: Harbor Lane Provisions - Wholesale specialty food Date: Thu, 15 Jan 2026 09:00:00 -0000 MIME-Version: 1.0 Content-Type: multipart/related; type="text/html"; boundary="----=_HarborLane_Archive_2026" ------=_HarborLane_Archive_2026 Content-Type: text/html; charset="utf-8" Content-Transfer-Encoding: 8bit Content-Location: https://harborlane.example/ 
 
 HYPERLINK "https://harborlane.example/" Home  HYPERLINK "https://harborlane.examp
[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/loading/load-specific-file-formats/load_document_with_format/load-document-with-format.txt)
{{< /tab >}}
{{< /tabs >}}

## Available file formats

The `FileType` enumeration includes the following formats:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser.options import FileType

# Common document formats (auto-detected)
# - PDF
# - DOC, DOCX
# - XLS, XLSX
# - PPT, PPTX
# - TXT
# - etc.

# Formats that may require explicit specification:
formats_needing_specification = {
    "Markdown": FileType.MD,
    "MHTML": FileType.MHTML,
    "OTP": FileType.OTP,
}

for name, format_value in formats_needing_specification.items():
    print(f"{name}: {format_value}")
```

{{< /tab >}}
{{< /tabs >}}

## Error handling when loading specific formats
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import LoadOptions

def safe_load_with_format(file_path, file_format):
    """Safely load a document with specific format and error handling"""
    
    try:
        # Create LoadOptions
        load_options = LoadOptions(file_format)
        
        # Create Parser instance
        with Parser(file_path, load_options) as parser:
            # Verify loading was successful
            doc_info = parser.get_document_info()
            print(f"Document loaded: {doc_info.file_type.file_format}")
            
            # Extract text
            text_reader = parser.get_text()
            if text_reader is not None:
                return text_reader.read_to_end()
            else:
                print("Text extraction not supported")
                return None
                
    except Exception as e:
        print(f"Error loading document with format {file_format}: {e}")
        
        # Try loading without format specification
        try:
            print("Attempting to load with auto-detection...")
            with Parser(file_path) as parser:
                text_reader = parser.get_text()
                if text_reader is not None:
                    return text_reader.read_to_end()
        except Exception as e2:
            print(f"Auto-detection also failed: {e2}")
            return None

# Example usage
from groupdocs.parser.options import FileType
text = safe_load_with_format("document.md", FileType.MD)
```

{{< /tab >}}
{{< tab "document.md" >}}
{{< tab-text >}}
The following sample file is used in this example: [document.md](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-specific-file-formats/document.md)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "safe-load-with-format.txt" >}}  
```text
Document loaded: Markdown Files
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/loading/load-specific-file-formats/safe_load_with_format/safe-load-with-format.txt)
{{< /tab >}}
{{< /tabs >}}

## When to use format specification

Use explicit format specification when:

1. **Working with Markdown files** - Always specify `FileType.MD` for `.md` files
2. **Processing MHTML web archives** - Specify `FileType.MHTML` for `.mhtml` or `.mht` files
3. **Loading OTP templates** - Specify `FileType.OTP` for OpenDocument presentation templates
4. **Connecting to databases** - Specify appropriate database format
5. **Fetching emails from remote servers** - Specify email format when loading from network

In most other cases, GroupDocs.Parser can automatically detect the document format based on file extension and content.

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Parser for Python via .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET)

### Free online document parser

Along with the full-featured library, we provide a free online document parser app. You are welcome to extract data from PDF, DOCX, XLSX, and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).

