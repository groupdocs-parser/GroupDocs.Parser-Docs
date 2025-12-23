---
id: load-password-protected-documents
url: parser/python-net/load-password-protected-documents
title: Load password-protected documents
weight: 3
description: "This article explains how to load password-protected PDF, Word, Excel, PowerPoint documents when using GroupDocs.Parser for Python via .NET."
keywords: Load password-protected document, Load protected document with GroupDocs.Parser, encrypted documents
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
    name: How to load password-protected documents in Python
    description: Learn how to load password-protected documents in Python step by step
    steps:
      - name: Create LoadOptions with password
        text: Create an object of LoadOptions which contains the password parameter.
      - name: Create Parser instance
        text: Create an instance of Parser class with the file path and LoadOptions object.
      - name: Extract data
        text: Use parser methods to extract text, images, metadata, or other data from the protected document.
---

[GroupDocs.Parser](https://products.groupdocs.com/parser/python-net) allows you to extract data from password-protected documents including encrypted PDFs and password-protected Office files.

To load a password-protected document, follow these steps:

1. Create a `LoadOptions` object and specify the document password
2. Instantiate the `Parser` object with the file path and `LoadOptions` object
3. Use extraction methods as usual

## Load password-protected document

The following code snippet shows how to load a password-protected document:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import LoadOptions

# Document password
password = "your-password"

# Create LoadOptions with the password
load_options = LoadOptions(password)

# Create an instance of Parser class with the file path and load options
with Parser("protected_document.pdf", load_options) as parser:
    # Extract text from the document
    text_reader = parser.get_text()
    
    if text_reader is not None:
        # Print the extracted text
        print(text_reader)
    else:
        print("Text extraction isn't supported for this format")
```

{{< /tab >}}
{{< tab "protected_document.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [protected_document.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-password-protected-documents/protected_document.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Handle incorrect password

You should handle cases when an incorrect password is provided:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import LoadOptions

def load_protected_document(file_path, password):
    """Load a password-protected document with error handling"""
    
    try:
        # Create LoadOptions with password
        load_options = LoadOptions(password)
        
        # Create Parser instance
        with Parser(file_path, load_options) as parser:
            # Get document info to verify successful loading
            doc_info = parser.get_document_info()
            
            print(f"Document loaded successfully!")
            print(f"Type: {doc_info.file_type.file_format}")
            print(f"Pages: {doc_info.page_count}")
            
            # Extract text
            text_reader = parser.get_text()
            if text_reader:
                return text_reader
            
    except Exception as e:
        print(f"Error loading document: {e}")
        return None

# Try loading with password
text = load_protected_document("protected.docx", "correct-password")
```

{{< /tab >}}
{{< tab "protected.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [protected.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-password-protected-documents/protected.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Check if document is password-protected

Before attempting to open a document, you can check if it requires a password:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import LoadOptions

def check_if_protected(file_path):
    """Check if a document is password-protected"""
    
    try:
        # Try to get file info without password
        file_info = Parser.get_file_info(file_path)
        
        # Check if the document is encrypted
        if hasattr(file_info, 'is_encrypted') and file_info.is_encrypted:
            print("Document is password-protected")
            return True
        else:
            print("Document is not password-protected")
            return False
            
    except Exception as e:
        print(f"Error checking document: {e}")
        return None

# Check if document is protected
is_protected = check_if_protected("sample.pdf")

if is_protected:
    password = input("Enter password: ")
    load_options = LoadOptions(password)
    with Parser("./sample.pdf", load_options) as parser:
        text_reader = parser.get_text()
        if text_reader:
            print(text_reader)
else:
    with Parser("./sample.pdf") as parser:
        text_reader = parser.get_text()
        if text_reader:
            print(text_reader)
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-password-protected-documents/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-password-protected-documents/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Load different protected formats
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import LoadOptions

def extract_from_protected(file_path, password, extract_type="text"):
    """Extract data from various password-protected formats"""
    
    try:
        # Create LoadOptions with password
        load_options = LoadOptions(password)
        
        # Create Parser instance
        with Parser(file_path, load_options) as parser:
            if extract_type == "text":
                # Extract text
                text_reader = parser.get_text()
                if text_reader:
                    return text_reader
                    
            elif extract_type == "metadata":
                # Extract metadata
                metadata = parser.get_metadata()
                if metadata:
                    result = {}
                    for item in metadata:
                        result[item.name] = item.value
                    return result
                    
            elif extract_type == "images":
                # Extract images
                images = parser.get_images()
                if images:
                    return list(images)
                    
    except Exception as e:
        print(f"Error: {e}")
        return None

# Extract text from protected PDF
text = extract_from_protected("protected.pdf", "password123", "text")
```

{{< /tab >}}
{{< tab "protected.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [protected.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/loading/load-password-protected-documents/protected.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Batch process protected documents
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import LoadOptions
import os

def process_protected_documents(directory, password):
    """Process all password-protected documents in a directory"""
    
    results = []
    
    for filename in os.listdir(directory):
        file_path = os.path.join(directory, filename)
        
        if not os.path.isfile(file_path):
            continue
        
        try:
            print(f"
Processing: {filename}")
            
            # Create LoadOptions with password
            load_options = LoadOptions(password)
            
            # Try to parse the document
            with Parser(file_path, load_options) as parser:
                # Get document info
                doc_info = parser.get_document_info()
                
                # Extract text
                text_reader = parser.get_text()
                text = text_reader if text_reader else ""
                
                results.append({
                    "filename": filename,
                    "type": doc_info.file_type.file_format,
                    "pages": doc_info.page_count,
                    "text_length": len(text),
                    "success": True
                })
                
                print(f"  ✓ Success - {len(text)} characters extracted")
                
        except Exception as e:
            print(f"  ✗ Failed - {e}")
            results.append({
                "filename": filename,
                "success": False,
                "error": str(e)
            })
    
    return results

# Process all protected documents
results = process_protected_documents("./protected_docs", "common-password")

# Print summary
successful = [r for r in results if r.get("success")]
print(f"
{len(successful)}/{len(results)} documents processed successfully")
```

{{< /tab >}}
{{< /tabs >}}

## Supported encryption types

GroupDocs.Parser supports the following encryption types:

- **PDF:** Standard password encryption (user password)
- **Office Documents:** Password-protected DOCX, XLSX, PPTX files
- **Legacy Office:** Password-protected DOC, XLS, PPT files

> **Note:** Documents with owner passwords (restrictions passwords) that only restrict printing/editing are not supported. Only documents with user passwords (opening passwords) can be processed.

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Parser for Python via .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET)

### Free online document parser

Along with the full-featured library, we provide a free online document parser app. You are welcome to extract data from PDF, DOCX, XLSX, and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).

