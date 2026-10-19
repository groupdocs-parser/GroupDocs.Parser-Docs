---
id: how-to-run-examples
url: parser/python-net/how-to-run-examples
title: How to Run Examples
weight: 6
description: "In this article, you can find how to run examples. We offer multiple solutions on how you can run GroupDocs.Parser examples, by building your own or using our back-end examples out-of-the-box."
keywords: How to run a parser, basic usage, How to run examples
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: True
structuredData:
  showOrganization: True
  application:
    name: Document Parser Tool
    description: The product allows to extract text, images, metadata, tables, and structured data from PDF, Word, Excel, PowerPoint and 60+ other file formats. Parser API also supports template-based extraction and container processing.
    productCode: parser
    productPlatform: python-net
  howTo:
    name: How to run Parser examples
    description: Learn how to run GroupDocs.Parser examples using Python
    steps:
      - name: Clone repository
        text: Clone the repository with Parser examples from GitHub
      - name: Install package
        text: Install GroupDocs.Parser package using pip
      - name: Run examples
        text: Execute run_all_examples.py to run the examples
---

The complete project [GroupDocs.Parser Examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET) with code examples and sample files is hosted on GitHub.

## Run examples using PyPI

To get started make sure that [Python](https://python.org/) is installed. The package supports Python 3.5-3.14; the examples use f-strings and need 3.6 or higher.

1. Clone repository with examples:
   ```bash
   git clone https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET.git
   ```

2. Navigate to the `Examples` folder:
   ```bash
   cd ./GroupDocs.Parser-for-Python-via-.NET/Examples
   ```

3. Install the necessary packages:
   ```bash
   pip install -r requirements.txt
   ```

4. Run the examples:
   ```bash
   python run_all_examples.py
   ```

Each example is a separate script in a folder that mirrors this documentation, next to its input files, so you can also run one on its own from its folder, for example `cd developer-guide/basic-usage/extract-text-from-documents` and then `python example-1.py`. Set `GROUPDOCS_LIC_PATH` to your licence file first to run them without the evaluation limits.

## Build project from scratch

If you prefer to create a project from scratch, follow these steps:

### Step 1: Install GroupDocs.Parser

Install GroupDocs.Parser for Python via .NET using pip:

```bash
pip install groupdocs-parser-net
```

### Step 2: Create a Python Script

Create a new Python file (e.g., `example.py`) and add the following code:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

# Create an instance of Parser class
with Parser("./sample.docx") as parser:
    # Extract text from the document
    text_reader = parser.get_text()

    if text_reader is None:
        print("Text extraction isn't supported for this format")
    else:
        # Print the extracted text
        print(text_reader.read_to_end())
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/getting-started/how-to-run-examples/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

### Step 3: Run the Script

Execute your Python script:

```bash
python example.py
```

The extracted text will appear in the console.

## Common Examples

### Extract Text from PDF
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("./sample.pdf") as parser:
    text_reader = parser.get_text()
    if text_reader is not None:
        print(text_reader.read_to_end())
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/getting-started/how-to-run-examples/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

### Extract Metadata
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("./sample.docx") as parser:
    metadata = parser.get_metadata()
    if metadata is not None:
        for item in metadata:
            print(f"{item.name}: {item.value}")
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/getting-started/how-to-run-examples/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

### Extract Images
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

with Parser("./sample.pdf") as parser:
    images = parser.get_images()
    if images is not None:
        print(f"Found {len(images)} images")
        for i, image in enumerate(images, start=1):
            # The extension includes the dot, e.g. ".jpeg"
            image.save(f"image_{i}{image.file_type.extension}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/getting-started/how-to-run-examples/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Contribute

If you like to add or improve an example, we encourage you to contribute to the project. All examples in this repository are open-source and can be freely used in your own applications.

To contribute, you can fork the repository, edit the source code and create a pull request. We will review the changes and include them in the repository if found helpful.
