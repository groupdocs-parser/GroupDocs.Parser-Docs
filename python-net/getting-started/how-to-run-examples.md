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
    description: The product allows to extract text, images, metadata, tables, and structured data from PDF, Word, Excel, PowerPoint and 50+ other file formats. Parser API also supports template-based extraction and container processing.
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
        text: Execute run_examples.py to run the examples
---

The complete project [GroupDocs.Parser Examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET) with code examples and sample files is hosted on GitHub.

## Run examples using PyPI

To get started make sure that [Python](https://python.org/) is installed (version 3.5 or higher).

1. Clone repository with examples:
   ```bash
   git clone https://github.com/groupdocs-parser/GroupDocs.Parser-for-Python-via-.NET.git
   ```

2. Navigate to the project folder:
   ```bash
   cd ./GroupDocs.Parser-for-Python-via-.NET
   ```

3. Install the necessary packages:
   ```bash
   pip install groupdocs-parser-net
   ```

4. Run the examples:
   ```bash
   python run_examples.py
   ```

To check what examples are available, open the `run_examples.py` file in your favorite text editor. Uncomment examples you want to run and type `python run_examples.py` to start them.

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

def extract_text_from_document():
    # Create an instance of Parser class
    with Parser("./sample.docx") as parser:
        # Extract text from the document
        text_reader = parser.get_text()
        
        if text_reader is not None:
            # Print the extracted text
            extracted_text = text_reader
            print(extracted_text)
        else:
            print("Text extraction isn't supported for this format")

if __name__ == "__main__":
    extract_text_from_document()
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

def extract_text_from_pdf():
    with Parser("./sample.pdf") as parser:
        text_reader = parser.get_text()
        if text_reader:
            print(text_reader)

if __name__ == "__main__":
    extract_text_from_pdf()
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

def extract_metadata_example():
    with Parser("./sample.docx") as parser:
        metadata = parser.get_metadata()
        if metadata:
            for item in metadata:
                print(f"{item.name}: {item.value}")

if __name__ == "__main__":
    extract_metadata_example()
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

def extract_images_example():
    with Parser("./sample.pdf") as parser:
        images = parser.get_images()
        if images:
            for i, image in enumerate(images):
                with open(f"image_{i}.png", "wb") as file:
                    image_stream = image.get_image_stream()
                    file.write(image_stream.read())

if __name__ == "__main__":
    extract_images_example()
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
