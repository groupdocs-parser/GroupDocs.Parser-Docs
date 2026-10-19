---
id: extract-text-structure
url: parser/python-net/extract-text-structure
title: Extract text structure
weight: 6
description: "Learn how to extract text structure from documents using GroupDocs.Parser for Python via .NET. Get XML representation of document structure including paragraphs, tables, lists."
keywords: extract text, extract text structure, document structure, XML structure
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser provides functionality to extract the text structure from documents as an XML representation, preserving document hierarchy including sections, paragraphs, tables, and lists.

## Prerequisites

- GroupDocs.Parser for Python via .NET installed
- Sample documents for testing
- Understanding of XML structure

## Document structure elements

The XML structure contains the following tags:

| Tag | Description |
|-----|-------------|
| `document` | The root tag |
| `section` | Represents a section (worksheet, slide, etc.). Attributes: `style`, `name` |
| `p` | Represents a text paragraph. Attribute: `style` |
| `ul` | Represents an unordered list |
| `ol` | Represents an ordered list |
| `li` | Represents a list item |
| `shape` | Represents a shape object |
| `table` | Represents a table |
| `tr` | Represents a table row |
| `td` | Represents a table cell. Attributes: `rowIndex`, `columnIndex`, `rowSpan`, `columnSpan` |
| `hyperlink` | Represents a hyperlink. Attribute: `link` |
| `strong` | Represents bold text |
| `em` | Represents italic text |
| `br` | Represents a line break |

## Extract text structure

To extract the document structure as XML:
{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import xml.etree.ElementTree as ET

# Create an instance of Parser class
with Parser("./sample.docx") as parser:
    # Extract text structure
    xml_reader = parser.get_structure()
    
    # Check if structure extraction is supported
    if xml_reader is None:
        print("Text structure extraction isn't supported")
    else:
        # Read XML content
        xml_reader.move_to_content()
        xml_content = xml_reader.read_outer_xml()
        print(xml_content)
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-structure/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Returns an XML representation of the document structure, or `None` if structure extraction is not supported.

## Parse and analyze document structure

To parse and analyze the XML structure:
{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import xml.etree.ElementTree as ET

# Create an instance of Parser class
with Parser("sample.docx") as parser:
    # Extract text structure
    xml_reader = parser.get_structure()
    
    if xml_reader is None:
        print("Structure extraction not supported")
    else:
        # Parse XML
        xml_reader.move_to_content()
        xml_content = xml_reader.read_outer_xml()
        root = ET.fromstring(xml_content)
        
        # Count elements
        sections = root.findall('.//section')
        paragraphs = root.findall('.//p')
        tables = root.findall('.//table')
        hyperlinks = root.findall('.//hyperlink')
        
        print(f"Document Structure Analysis:")
        print(f"  Sections: {len(sections)}")
        print(f"  Paragraphs: {len(paragraphs)}")
        print(f"  Tables: {len(tables)}")
        print(f"  Hyperlinks: {len(hyperlinks)}")
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-structure/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Parses the XML structure and provides statistics about document elements.

## Extract all hyperlinks from structure

To extract hyperlinks using the structure:
{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import xml.etree.ElementTree as ET

def extract_hyperlinks_from_structure(file_path):
    """
    Extract all hyperlinks using document structure.
    """
    with Parser(file_path) as parser:
        xml_reader = parser.get_structure()
        
        if xml_reader is None:
            print("Structure extraction not supported")
            return []
        
        # Parse XML
        xml_reader.move_to_content()
        xml_content = xml_reader.read_outer_xml()
        root = ET.fromstring(xml_content)
        
        # Find all hyperlink elements
        hyperlinks = []
        for hyperlink in root.findall('.//hyperlink'):
            url = hyperlink.get('link', '')
            text = ''.join(hyperlink.itertext())
            
            if url:
                hyperlinks.append({
                    'text': text,
                    'url': url
                })
        
        return hyperlinks

# Usage
links = extract_hyperlinks_from_structure("sample.pdf")
print(f"Found {len(links)} hyperlinks:\n")
for link in links:
    print(f"  {link['text']} -> {link['url']}")
```

{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-structure/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "extract-hyperlinks-from-structure.txt" >}}  
```text
Structure extraction not supported
Found 0 hyperlinks:
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/extraction/extract-text-structure/extract_hyperlinks_from_structure/extract-hyperlinks-from-structure.txt)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts all hyperlinks with their text and URLs from the document structure.

## Extract table structure

To analyze table structure:
{{< tabs "example-4" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import xml.etree.ElementTree as ET

def analyze_tables(file_path):
    """
    Analyze table structure in document.
    """
    with Parser(file_path) as parser:
        xml_reader = parser.get_structure()
        
        if xml_reader is None:
            print("Structure extraction not supported")
            return
        
        # Parse XML
        xml_reader.move_to_content()
        xml_content = xml_reader.read_outer_xml()
        root = ET.fromstring(xml_content)
        
        # Find all tables
        tables = root.findall('.//table')
        
        print(f"Found {len(tables)} tables:\n")
        
        for idx, table in enumerate(tables, 1):
            rows = table.findall('.//tr')
            
            print(f"Table {idx}:")
            print(f"  Rows: {len(rows)}")
            
            # Get column count from first row (a merged cell counts its columnSpan)
            if rows:
                cells = rows[0].findall('td')
                print(f"  Columns: {sum(int(td.get('columnSpan', '1')) for td in cells)}")
            
            # Show cell spanning (cells that span more than one row or column)
            cells_with_span = [td for td in table.findall('.//td')
                               if td.get('rowSpan', '1') != '1' or td.get('columnSpan', '1') != '1']
            if cells_with_span:
                print(f"  Cells with spanning: {len(cells_with_span)}")
            
            print()

# Usage
analyze_tables("sample.xlsx")
```

{{< /tab >}}
{{< tab "sample.xlsx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.xlsx](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-structure/sample.xlsx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "analyze-tables.txt" >}}  
```text
Found 2 tables:

Table 1:
  Rows: 41
  Columns: 9

Table 2:
  Rows: 12
  Columns: 4
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/extraction/extract-text-structure/analyze_tables/analyze-tables.txt)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Provides detailed analysis of table structures including rows, columns, and cell spanning.

## Extract formatted text

To extract text with formatting information:
{{< tabs "example-5" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import xml.etree.ElementTree as ET

def extract_formatted_text(file_path):
    """
    Extract text with formatting (bold, italic).
    """
    with Parser(file_path) as parser:
        xml_reader = parser.get_structure()
        
        if xml_reader is None:
            print("Structure extraction not supported")
            return
        
        # Parse XML
        xml_reader.move_to_content()
        xml_content = xml_reader.read_outer_xml()
        root = ET.fromstring(xml_content)
        
        # Find formatted text
        bold_texts = root.findall('.//strong')
        italic_texts = root.findall('.//em')
        
        print("Bold text:")
        for text in bold_texts:
            content = ''.join(text.itertext()).strip()
            if content:
                print(f"  - {content}")
        
        print("\nItalic text:")
        for text in italic_texts:
            content = ''.join(text.itertext()).strip()
            if content:
                print(f"  - {content}")

# Usage
extract_formatted_text("sample.docx")
```

{{< /tab >}}
{{< tab "sample.docx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.docx](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-structure/sample.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "extract-formatted-text.txt" >}}  
```text
Bold text:
  - Supply Agreement
  - 1. Parties
  - Buyer:
  - Supplier:
  - 2.1 Autumn Harvest allocation
  - 3. Prices and fees
  - Product
  - Unit
  - Price
[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/extraction/extract-text-structure/extract_formatted_text/extract-formatted-text.txt)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Extracts all bold and italic text from the document.

## Save structure to file

To save the XML structure to a file:
{{< tabs "example-6" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser

def save_structure_to_file(file_path, output_xml):
    """
    Extract and save document structure to XML file.
    """
    with Parser(file_path) as parser:
        xml_reader = parser.get_structure()
        
        if xml_reader is None:
            print("Structure extraction not supported")
            return False
        
        # Read and save XML
        xml_reader.move_to_content()
        xml_content = xml_reader.read_outer_xml()
        
        with open(output_xml, 'w', encoding='utf-8') as f:
            f.write(xml_content)
        
        print(f"Structure saved to {output_xml}")
        return True

# Usage
save_structure_to_file("sample.pdf", "structure.xml")
```

{{< /tab >}}
{{< tab "save-structure.txt" >}}  
```text
Structure extraction not supported
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/extraction/extract-text-structure/save_structure_to_file/save-structure.txt)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Saves the document structure as an XML file for further processing.

## Extract section information

To extract information about document sections:
{{< tabs "example-7" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
import xml.etree.ElementTree as ET

def extract_section_info(file_path):
    """
    Extract section information from document.
    """
    with Parser(file_path) as parser:
        xml_reader = parser.get_structure()
        
        if xml_reader is None:
            print("Structure extraction not supported")
            return
        
        # Parse XML
        xml_reader.move_to_content()
        xml_content = xml_reader.read_outer_xml()
        root = ET.fromstring(xml_content)
        
        # Find all sections
        sections = root.findall('.//section')
        
        print(f"Document has {len(sections)} sections:\n")
        
        for idx, section in enumerate(sections, 1):
            name = section.get('name', f'Section {idx}')
            style = section.get('style', 'default')
            
            # Count elements in section
            paragraphs = section.findall('.//p')
            tables = section.findall('.//table')
            lists = section.findall('.//ul') + section.findall('.//ol')
            
            print(f"Section: {name}")
            print(f"  Style: {style}")
            print(f"  Paragraphs: {len(paragraphs)}")
            print(f"  Tables: {len(tables)}")
            print(f"  Lists: {len(lists)}")
            print()

# Usage
extract_section_info("sample.pptx")
```

{{< /tab >}}
{{< tab "sample.pptx" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pptx](/parser/python-net/_sample_files/developer-guide/advanced-usage/extraction/extract-text-structure/sample.pptx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "extract-section-info.txt" >}}  
```text
Document has 3 sections:

Section: Section 1
  Style: default
  Paragraphs: 2
  Tables: 0
  Lists: 0

Section: Section 2
  Style: default
[TRUNCATED]
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/extraction/extract-text-structure/extract_section_info/extract-section-info.txt)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Provides detailed information about each section in the document.

## Notes

- The `get_structure()` method returns `None` if structure extraction is not supported
- XML structure varies by document type (Word, Excel, PowerPoint)
- Use Python's `xml.etree.ElementTree` or other XML parsers to process the structure
- Structure extraction preserves document hierarchy and formatting information
- Useful for document analysis, content extraction, and format conversion
- Different document types have specific structure features (see documentation)

## Related pages

- [Extract text in Accurate mode]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-in-accurate-mode" >}})
- [Extract text areas]({{< ref "parser/python-net/developer-guide/advanced-usage/extraction/extract-text-areas" >}})
- [Extract hyperlinks from document]({{< ref "parser/python-net/developer-guide/advanced-usage/working-with-hyperlinks/extract-hyperlinks-from-document" >}})

