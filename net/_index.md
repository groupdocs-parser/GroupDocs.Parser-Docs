---
id: home
url: parser/net
title: GroupDocs.Parser for .NET
weight: 1
description: "A convenient text extractor API that permits users to extract raw or formatted text from different document formats. Besides, it is not only a text extractor API, the user can extract metadata from the document as well. "
keywords: parser api, text extractor, extract raw, extract metadata
productName: GroupDocs.Parser for .NET
hideChildren: True
---
{{< alert style="info" >}}
![](/parser/net/images/home.png) **Welcome to the GroupDocs.Parser for .NET**  
GroupDocs.Parser is a convenient text extractor API that permits users to extract raw or formatted text from different document formats. Besides, it is not only a text extractor API, the user can extract metadata from the document as well.
{{< /alert >}}


# GroupDocs.Parser for .NET

## Overview

GroupDocs.Parser for .NET is a powerful document data extraction API that enables developers to parse and extract information from over 50 document types. This robust library allows you to extract text, metadata, images, tables, and other structured data from various file formats without requiring additional software installations.

### Key Highlights

- **50+ Document Formats** - Support for PDF, Office documents, images, archives, and more
- **Template-Based Parsing** - Extract structured data using predefined templates
- **Multiple Extraction Modes** - Raw text, formatted text, and precise data extraction
- **Enterprise Ready** - Scalable solution for high-volume document processing
- **Cross-Platform** - Compatible with .NET Framework, .NET Core, and .NET 5+

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

Install via NuGet Package Manager:

```powershell
Install-Package GroupDocs.Parser
```

Or via .NET CLI:
```bash
dotnet add package GroupDocs.Parser
```

## Quick Start Example

```csharp
using GroupDocs.Parser;

// Create an instance of Parser class
using (Parser parser = new Parser("sample.pdf"))
{
    // Extract text from the document
    using (TextReader reader = parser.GetText())
    {
        // Check if text extraction is supported
        if (reader != null)
        {
            // Print the extracted text
            Console.WriteLine(reader.ReadToEnd());
        }
        else
        {
            Console.WriteLine("Text extraction isn't supported for this format");
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
- Extract hyperlinks and bookmarks

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

[View Complete List of Supported Formats →]({{< ref "parser/net/getting-started/supported-document-formats.md" >}})

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

```csharp
// Create a template with fixed positions
Template template = new Template(new TemplateItem[]
{
    new TemplateField(new TemplateFixedPosition(new Rectangle(new Point(35, 135), new Size(100, 10))), "CompanyName"),
    new TemplateField(new TemplateFixedPosition(new Rectangle(new Point(35, 150), new Size(100, 10))), "InvoiceNumber")
});

// Parse document using template
using (Parser parser = new Parser("invoice.pdf"))
{
    DocumentData data = parser.ParseByTemplate(template);
    // Process extracted data
}
```

### Batch Processing
Process multiple documents efficiently:

```csharp
string[] files = Directory.GetFiles(@"C:\Documents", "*.*");
foreach (string file in files)
{
    using (Parser parser = new Parser(file))
    {
        // Extract and process data from each file
        using (TextReader reader = parser.GetText())
        {
            if (reader != null)
            {
                string content = reader.ReadToEnd();
                // Process the content
            }
        }
    }
}
```

## Performance & Scalability

- **High Performance** - Optimized for processing large documents
- **Memory Efficient** - Stream-based processing for large files
- **Thread Safe** - Support for multi-threaded applications
- **Scalable Architecture** - Handle high-volume document processing

## Getting Started Resources

### Sample Code & Examples
- [GitHub Repository](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET) - Complete source code examples
- [Live Demos](https://products.groupdocs.app/parser/net) - Try the API online

### API Reference & Support
- [API Reference](https://reference.groupdocs.com/net/parser) - Complete API documentation
- [Forum Support](https://forum.groupdocs.com/c/parser) - Community support and discussions

## Licensing & Trial

- [Free Trial](https://purchase.groupdocs.com/temporary-license/) - Evaluate all features with temporary license
- [Licensing Options](https://purchase.groupdocs.com/policies/use-license/) - Flexible licensing for different use cases
- [Purchase](https://purchase.groupdocs.com/buy) - Get your production license

## System Requirements

- **.NET Framework** 4.6.1 or higher
- **.NET Core** 2.0 or higher  
- **.NET** 5.0 or higher
- **Operating Systems** - Windows, Linux, macOS
- **Development Environments** - Visual Studio, VS Code, JetBrains Rider

---

## GroupDocs.Parser for .NET Resources
Following are the links to some useful resources you may need to accomplish your tasks.
*   [GroupDocs.Parser for .NET Online Documentation]({{< ref "parser/net" >}})
*   [GroupDocs.Parser for .NET Features]({{< ref "parser/net/getting-started/features-overview.md" >}})
*   [GroupDocs.Parser for .NET Limitations]({{< ref "parser/net/getting-started/evaluation-limitations-and-licensing.md" >}})
*   [GroupDocs.Parser for .NET Release Notes](https://releases.groupdocs.com/parser/net/release-notes/)
*   [GroupDocs.Parser for .NET Product Page](https://products.groupdocs.com/parser/net)
*   [Install GroupDocs.Parser for .NET NuGet Package](https://www.nuget.org/packages/GroupDocs.Parser/)
*   [GroupDocs.Parser for .NET API Reference Guide](https://reference.groupdocs.com/net/parser)
*   [GroupDocs.Parser for .NET Free Support Forum](https://forum.groupdocs.com/c/parser)
*   [GroupDocs.Parser for .NET Paid Support Helpdesk](https://helpdesk.groupdocs.com/)


**Ready to start extracting data from your documents?** [Download the free trial](https://purchase.groupdocs.com/temporary-license/) and begin parsing documents in minutes!