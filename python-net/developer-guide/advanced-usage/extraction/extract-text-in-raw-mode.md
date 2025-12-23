---
id: extract-text-in-raw-mode
url: parser/python-net/extract-text-in-raw-mode
title: Extract text in Raw mode
weight: 2
description: "Learn how to extract text in Raw mode from documents using GroupDocs.Parser for Python via .NET. Fast text extraction with improved performance for large documents."
keywords: extract text, extract text in Raw mode, raw text extraction, fast text extraction, performance text extraction
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser provides **Raw mode** for text extraction, which offers significantly faster processing speed at the cost of formatting accuracy. This mode is ideal for large documents where speed is more important than perfect layout preservation.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample documents for testing
- Understanding of text extraction modes

## Accurate vs Raw mode

| Feature | Accurate Mode | Raw Mode |
|---------|--------------|----------|
| **Speed** | Slower | Faster |
| **Formatting** | Best quality | Basic quality |
| **Layout** | Preserved | May be altered |
| **Use Case** | High-quality output | High-speed processing |

## Extract text in Raw mode

To extract text from a document in Raw mode:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import TextOptions

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Create text options for raw mode
    options = TextOptions(False)  # False = Raw mode, True = Accurate mode
    
    # Extract text in raw mode
    text_reader = parser.get_text(options)
    
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
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-in-raw-mode/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns text extracted in Raw mode with improved performance but potentially reduced formatting quality.

## Extract text from page in Raw mode

To extract text from a specific page in Raw mode:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import TextOptions

# Create an instance of Parser class
with Parser("./sample.pdf") as parser:
    # Get document info
    info = parser.get_document_info()
    
    # Check if document has pages
    if info.raw_page_count == 0:
        print("Document has no pages")
        return
    
    # Create text options for raw mode
    options = TextOptions(False)
    
    # Iterate over pages
    for page_index in range(info.raw_page_count):
        print(f"
Page {page_index + 1}/{info.raw_page_count}")
        
        # Extract text from page in raw mode
        text_reader = parser.get_text(page_index, options)
        
        if text_reader is not None:
            print(text_reader)
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-in-raw-mode/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts text page by page in Raw mode, using `raw_page_count` for accurate page enumeration.

## Important note about page counts

When using Raw mode, always use `raw_page_count` instead of `page_count`:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import TextOptions

# Create an instance of Parser class
with Parser("sample.pdf") as parser:
    # Get document info
    info = parser.get_document_info()
    
    print(f"Accurate mode pages: {info.page_count}")
    print(f"Raw mode pages: {info.raw_page_count}")
    
    # The page count may differ between modes!
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-in-raw-mode/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Raw mode may have a different page count than Accurate mode due to different text layout algorithms.

## Performance comparison

Compare extraction speed between modes:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import TextOptions
import time

def compare_extraction_modes(file_path):
    """
    Compare performance between Accurate and Raw modes.
    """
    # Accurate mode
    start_time = time.time()
    with Parser(file_path) as parser:
        text_reader = parser.get_text()
        if text_reader:
            accurate_text = text_reader
            accurate_time = time.time() - start_time
            accurate_length = len(accurate_text)
        else:
            print("Text extraction not supported")
            return
    
    # Raw mode
    start_time = time.time()
    with Parser(file_path) as parser:
        options = TextOptions(False)
        text_reader = parser.get_text(options)
        if text_reader:
            raw_text = text_reader
            raw_time = time.time() - start_time
            raw_length = len(raw_text)
    
    # Print comparison
    print(f"Performance Comparison:")
    print(f"{'Mode':<15} {'Time (s)':<15} {'Length':<15} {'Speed':<15}")
    print("-" * 60)
    print(f"{'Accurate':<15} {accurate_time:<15.3f} {accurate_length:<15} {'1.0x':<15}")
    print(f"{'Raw':<15} {raw_time:<15.3f} {raw_length:<15} {accurate_time/raw_time:.2f}x faster")

# Usage
compare_extraction_modes("sample.pdf")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-in-raw-mode/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Demonstrates the speed improvement of Raw mode over Accurate mode.

## Batch processing with Raw mode

Process multiple documents efficiently:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import TextOptions
from pathlib import Path
import time

def batch_extract_raw(input_dir, output_dir):
    """
    Extract text from multiple documents using Raw mode for speed.
    """
    output_path = Path(output_dir)
    output_path.mkdir(exist_ok=True)
    
    extensions = ['.pdf', '.docx', '.doc', '.txt']
    
    start_time = time.time()
    processed_count = 0
    
    # Create raw mode options
    options = TextOptions(False)
    
    for file_path in Path(input_dir).rglob('*'):
        if file_path.suffix.lower() in extensions:
            print(f"Processing: {file_path.name}")
            
            try:
                with Parser(str(file_path)) as parser:
                    text_reader = parser.get_text(options)
                    
                    if text_reader:
                        # Save extracted text
                        output_file = output_path / f"{file_path.stem}.txt"
                        with open(output_file, 'w', encoding='utf-8') as f:
                            f.write(text_reader)
                        
                        processed_count += 1
                
            except Exception as e:
                print(f"  Error: {e}")
    
    elapsed_time = time.time() - start_time
    print(f"
Processed {processed_count} documents in {elapsed_time:.2f} seconds")
    print(f"Average: {elapsed_time/processed_count:.2f} seconds per document")

# Usage
batch_extract_raw("input_documents", "extracted_text")
```

{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Quickly processes multiple documents using Raw mode for maximum throughput.

## When to use Raw mode

**Use Raw mode when:**
- Processing large volumes of documents
- Speed is more important than formatting
- Extracting text for indexing or search
- Layout preservation is not critical
- Working with simple text documents

**Use Accurate mode when:**
- Formatting quality is important
- Layout must be preserved
- Extracting structured data
- Processing invoices or forms
- Output will be displayed to users

## Extract and index text (search use case)

Raw mode is ideal for creating search indexes:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.options import TextOptions
from pathlib import Path
import json

def create_search_index(input_dir, index_file):
    """
    Create a search index from documents using Raw mode.
    """
    index = {}
    options = TextOptions(False)  # Raw mode for speed
    
    for file_path in Path(input_dir).rglob('*'):
        if file_path.suffix.lower() in ['.pdf', '.docx', '.doc', '.txt']:
            try:
                with Parser(str(file_path)) as parser:
                    text_reader = parser.get_text(options)
                    
                    if text_reader:
                        text = text_reader
                        
                        # Create index entry
                        index[str(file_path)] = {
                            'filename': file_path.name,
                            'size': len(text),
                            'preview': text[:200],  # First 200 chars
                            'word_count': len(text.split())
                        }
                        
                        print(f"Indexed: {file_path.name}")
            
            except Exception as e:
                print(f"Error indexing {file_path.name}: {e}")
    
    # Save index
    with open(index_file, 'w', encoding='utf-8') as f:
        json.dump(index, f, indent=2, ensure_ascii=False)
    
    print(f"
Created index with {len(index)} documents")
    return index

# Usage
index = create_search_index("documents", "search_index.json")
```

{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Creates a lightweight search index using fast Raw mode extraction.

## Notes

- Raw mode is typically 2-5x faster than Accurate mode
- Use `raw_page_count` instead of `page_count` in Raw mode
- Text layout and formatting may differ from the original document
- Raw mode returns `None` for unsupported documents (same as Accurate mode)
- Some documents may have different page numbers in Raw vs Accurate mode
- Raw mode is still accurate for plain text content, just not for complex layouts

## Related pages

- [Extract text in Accurate mode]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-in-accurate-mode" >}})
- [Search text in documents]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/search-text" >}})
- [Get document info]({{< ref "parser/python-net/developer-guide/basic-usage/get-document-info" >}})

