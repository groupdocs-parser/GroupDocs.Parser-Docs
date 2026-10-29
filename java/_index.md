---
id: home
url: parser/java
title: GroupDocs.Parser for Java
weight: 1
version: 26.10
description: "A convenient text extractor API that permits users to extract raw or formatted text from different document formats. Besides, it is not only a text extractor API, the user can extract metadata, images and tables from the document as well."
keywords: parser api, text extractor, extract raw, extract metadata, PDF text extraction library Java, extract text from PDF Java, Java document parser
productName: GroupDocs.Parser for Java
hideChildren: True
---
{{< alert style="info" >}}
<img src="/logo/128x128/groupdocs-parser-java.png" alt="groupdocs-parser-java-home" align="left" style="width:110px; margin: 0 30px 30px 0"/> **Welcome to the GroupDocs.Parser for Java**  
GroupDocs.Parser is a convenient text extractor API that permits users to extract raw or formatted text from different document formats. Besides, the user can extract metadata from the document.
{{< /alert >}}


# GroupDocs.Parser for Java

## Overview

GroupDocs.Parser for Java is a powerful **PDF text extraction library for Java** that enables developers to parse and extract information from over 65 document types. This library allows you to **extract text from PDF** files, Word documents, Excel spreadsheets, and more. You can extract raw or formatted text, metadata, images, tables, and other structured data from various file formats. The library works without requiring additional software installations; OCR of scanned documents is available through an OCR connector that you plug in (see [OCR usage]({{< ref "parser/java/developer-guide/advanced-usage/using-ocr/_index.md" >}})).

### Key Highlights

- **65+ document formats** - Support for PDF, Office documents, images, archives, and more
- **Template-Based Parsing** - Extract structured data using predefined templates
- **Multiple Extraction Modes** - Raw text, formatted text, and precise data extraction
- **Enterprise Ready** - Scalable solution for high-volume document processing
- **Cross-Platform** - Runs on Java 8 or later on Windows, Linux and macOS

## What Makes GroupDocs.Parser Unique?

### Template-Driven Extraction
One of the most powerful features is parsing documents with predefined templates. Easily define templates to extract specific data from invoices, receipts, contracts, and other structured documents with high precision.

### Versatile Text Extraction
- **Raw Text Mode** - Fast extraction for basic text processing
- **Formatted Text Mode** - Preserve document formatting and structure
- **Search & Highlight** - Advanced text search with regex support and keyword highlighting

### Comprehensive Data Extraction
- Extract images and save them in various formats
- Retrieve document metadata (author, creation date, etc.)
- Parse tables and maintain their structure
- Extract attachments from containers and email files
- Process PDF forms and fillable fields

## Installation

### Maven

Add the GroupDocs repository and the dependency to your `pom.xml`:

```xml
<repositories>
    <repository>
        <id>GroupDocsJavaAPI</id>
        <name>GroupDocs Java API</name>
        <url>https://releases.groupdocs.com/java/repo/</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>com.groupdocs</groupId>
        <artifactId>groupdocs-parser</artifactId>
        <version>26.10</version>
    </dependency>
</dependencies>
```

### Gradle

```groovy
repositories {
    mavenCentral()
    maven { url "https://releases.groupdocs.com/java/repo/" }
}

dependencies {
    implementation "com.groupdocs:groupdocs-parser:26.10"
}
```

### Prerequisites

**Java:** Java 8 (1.8) or later.

**For Linux:** No native packages are required. Install TrueType fonts (for example, `ttf-mscorefonts-installer`) if you generate page previews or extract text areas.

See the [Installation Guide]({{< ref "parser/java/getting-started/installation.md" >}}) for detailed setup instructions.

## Quick Start Example

**Extract text from PDF in Java:**

```java
import com.groupdocs.parser.Parser;
import com.groupdocs.parser.data.TextReader;

// Create an instance of Parser class
try (Parser parser = new Parser("sample.pdf")) {
    // Extract text from the document
    try (TextReader reader = parser.getText()) {
        // Check if text extraction is supported
        if (reader != null) {
            // Print the extracted text
            System.out.println(reader.readToEnd());
        } else {
            System.out.println("Text extraction isn't supported for this format");
        }
    }
}
```

## Core Features

### Text Extraction
- Raw and formatted text extraction
- Page-wise text extraction
- Text areas extraction with coordinates
- Search text with advanced options (case sensitivity, whole words, regex)

### Metadata & Document Information
- Extract document properties (title, author, subject, etc.)
- Retrieve file format information
- Get page count and document structure
- Extract custom metadata fields

### Image & Media Extraction
- Extract images from documents
- Support for multiple image formats (JPEG, PNG, GIF, BMP)
- Preserve image quality and dimensions
- Extract images with position information

### Structured Data Extraction
- Parse tables with preserved formatting
- Extract data using custom templates
- Process PDF forms and fillable fields
- Extract hyperlinks and barcodes

## Supported Document Formats

| Category | Formats |
|----------|---------|
| **Documents** | PDF, DOC, DOCX, RTF, TXT, ODT, EPUB |
| **Spreadsheets** | XLS, XLSX, ODS, CSV |
| **Presentations** | PPT, PPTX, ODP |
| **Emails & Archives** | MSG, EML, PST, OST, ZIP, RAR, TAR |
| **Images** | JPEG, PNG, GIF, BMP, TIFF |
| **Web & Markup** | HTML, XHTML, XML |
| **eBooks** | EPUB, FB2, CHM |

[View Complete List of Supported Formats →]({{< ref "parser/java/getting-started/supported-document-formats.md" >}})

## Use Cases & Applications

### Enterprise Document Management
- **Full-Text Search Integration** - Build searchable document repositories
- **Content Indexing** - Extract and index document content for search engines
- **Document Classification** - Categorize documents based on extracted content

### Legal & Compliance
- **eDiscovery Solutions** - Extract and analyze text from legal documents
- **Contract Analysis** - Parse contracts for key terms and clauses
- **Regulatory Compliance** - Extract data for compliance reporting

### Financial Services
- **Invoice Processing** - Parse invoices, receipts, and financial documents
- **Automated Data Entry** - Extract structured data from forms
- **Financial Document Analysis** - Process statements and reports

### Data Migration & Integration
- **Legacy System Migration** - Extract content from old document formats
- **System Integration** - Parse documents for API integrations
- **Data Transformation** - Convert unstructured data to structured formats

## Advanced Features

### Template-Based Data Extraction
Create custom templates to extract specific data fields:

```java
// Create a template with fixed positions
Template template = new Template(java.util.Arrays.asList(
        new TemplateField(new TemplateFixedPosition(new Rectangle(new Point(35, 135), new Size(100, 10))), "CompanyName"),
        new TemplateField(new TemplateFixedPosition(new Rectangle(new Point(35, 150), new Size(100, 10))), "InvoiceNumber")
));

// Parse document using template
try (Parser parser = new Parser("invoice.pdf")) {
    DocumentData data = parser.parseByTemplate(template);
    // Process extracted data
}
```

See [Working with templates]({{< ref "parser/java/developer-guide/advanced-usage/working-with-templates.md" >}}) for details.

### Batch Processing
Process multiple documents efficiently:

```java
File[] files = new File("C:\\Documents").listFiles(File::isFile);
if (files != null) {
    for (File file : files) {
        try (Parser parser = new Parser(file.getPath())) {
            // Extract and process data from each file
            try (TextReader reader = parser.getText()) {
                if (reader != null) {
                    String content = reader.readToEnd();
                    // Process the content
                }
            }
        } catch (UnsupportedDocumentFormatException ex) {
            // Skip files of unsupported formats
        }
    }
}
```

## Performance & Scalability

- **High Performance** - Raw text extraction mode for performance-critical scenarios
- **Memory Efficient** - Load documents from an `InputStream` and read text through a `TextReader`
- **Scalable Architecture** - Handle high-volume document processing
- **On-Premise** - Documents never leave your infrastructure (see [Network access and data privacy]({{< ref "parser/java/getting-started/network-access-and-data-privacy.md" >}}))

## Getting Started Resources

### Sample Code & Examples
- [GitHub Repository](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java) - Complete source code examples
- [Live Demos](https://products.groupdocs.app/parser/family) - Try the API online

### API Reference & Support
- [API Reference](https://reference.groupdocs.com/parser/java/) - Complete API documentation
- [Forum Support](https://forum.groupdocs.com/c/parser) - Community support and discussions

## Licensing & Trial

- [Free Trial](https://purchase.groupdocs.com/temp-license/100308) - Evaluate all features with temporary license
- [Licensing Options](https://purchase.groupdocs.com/policies/use-license/) - Flexible licensing for different use cases
- [Purchase](https://purchase.groupdocs.com/buy) - Get your production license

## System Requirements

- **Java** 8 (1.8) or later
- **Operating Systems** - Windows, Linux, macOS
- **Build Tools** - Maven, Gradle
- **Development Environments** - IntelliJ IDEA, Eclipse, NetBeans, VS Code

See [System Requirements]({{< ref "parser/java/getting-started/system-requirements.md" >}}) for details.

---

## GroupDocs.Parser for Java Resources
Following are the links to some useful resources you may need to accomplish your tasks.
*   [GroupDocs.Parser for Java Online Documentation]({{< ref "parser/java" >}})
*   [GroupDocs.Parser for Java Features]({{< ref "parser/java/getting-started/features-overview.md" >}})
*   [GroupDocs.Parser for Java Limitations]({{< ref "parser/java/getting-started/evaluation-limitations-and-licensing.md" >}})
*   [GroupDocs.Parser for Java Release Notes](https://releases.groupdocs.com/parser/java/release-notes/)
*   [GroupDocs.Parser for Java Product Page](https://products.groupdocs.com/parser/java)
*   [GroupDocs.Parser for Java Releases](https://releases.groupdocs.com/parser/java/)
*   [GroupDocs.Parser for Java Maven Repository](https://releases.groupdocs.com/java/repo/com/groupdocs/groupdocs-parser/)
*   [GroupDocs.Parser for Java API Reference Guide](https://reference.groupdocs.com/parser/java/)
*   [GroupDocs.Parser for Java Free Support Forum](https://forum.groupdocs.com/c/parser)
*   [GroupDocs.Parser for Java Paid Support Helpdesk](https://helpdesk.groupdocs.com/)


**Ready to start extracting data from your documents?** [Download the free trial](https://purchase.groupdocs.com/temp-license/100308) and begin parsing documents in minutes!
