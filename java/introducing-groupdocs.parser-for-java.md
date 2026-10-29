---
id: introducing-groupdocs-parser-for-java
url: parser/java/introducing-groupdocs-parser-for-java
title: Introducing GroupDocs.Parser for Java
weight: 1
description: "Java document parsing API for extracting text, images, metadata, tables and structured data from 65+ file formats including PDF, Word, Excel, PowerPoint. Features template-based extraction, full-text search, and enterprise-ready document processing capabilities."
keywords: "GroupDocs.Parser, Java document parser, text extraction API, PDF parser, document data extraction, template parsing, metadata extraction, Java document processing, file format parser, enterprise document management, text mining, data extraction library, PDF text extraction library Java, extract text from PDF Java, extract tables from Excel Java"
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---
## What Is GroupDocs.Parser?

GroupDocs.Parser is a powerful document data extraction API from over 65 document types in your applications.
Many popular formats are supported: PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, ODT, ODS, RTF, EPUB and many others.
  
One of the most valuable features of GroupDocs.Parser is parsing documents with predefined templates. It's easy to define template and extract data from invoices, prices or other kinds of your typical documents.  

The API allows to easily extract text in accurate and quick modes. There are several advanced methods to extract text.  

The API also provides methods to extract images, extract metadata. You can do it with regular documents and containers like ZIP archives, OST/PST mail data files and PDF portfolios.  

If you want to extract PDF forms, GroupDocs.Parser also allows to do it.

## Why Use GroupDocs.Parser?

*   No additional software is required to extract text, metadata, and images from PDF, Office documents, and 65+ formats out of the box. Note: GroupDocs.Parser for Java has no built-in OCR; to extract text from scanned documents and images, plug in an OCR library of your choice through an OCR connector (see [OCR usage]({{< ref "parser/java/developer-guide/advanced-usage/using-ocr/_index.md" >}}));
*   Online free document data extraction App for simple cases and powerful Java library for many data extraction scenarios;
*   Accurate and fast Raw text extraction modes;
*   Document information extraction - file type, page count etc;
*   Metadata extraction;
*   Images extraction;
*   Attachments extraction;
*   Parsing documents by user-generated templates;
*   Parsing PDF forms.

## Key Features

- Extract raw text and formatted text.  
- Search by keywords with advanced options (case sensitivity, whole word match, regex).  
- Work with document metadata and structured elements.  
- Extract images, attachments, and tables.  
- Use parsing templates to extract data fields with precision.  

## Getting Started

### Installation

Add **GroupDocs.Parser for Java** to your Maven project from the GroupDocs Repository:

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

See [Installation]({{< ref "parser/java/getting-started/installation.md" >}}) for Gradle and other options.

### Simple Example – Extract Text

```java
// Create an instance of Parser class
try (Parser parser = new Parser(filePath)) {
    // Extract a text into the reader
    try (TextReader reader = parser.getText()) {
        // Print a text from the document
        // If text extraction isn't supported, a reader is null
        System.out.println(reader == null ? "Text extraction isn't supported" : reader.readToEnd());
    }
}
```

# Document Processing Guide

## Use Cases

* **Enterprise Search Systems** – Integrate full-text search in internal document management.
* **eDiscovery & Legal** – Extract and analyze text from legal files.
* **Financial Applications** – Parse invoices, receipts, and structured data using templates.
* **Data Migration** – Extract structured content for migration between document systems.

## Supported Formats

| Category | Formats |
|----------|---------|
| Documents | PDF, DOC, DOCX, RTF, TXT, ODT, EPUB |
| Spreadsheets | XLS, XLSX, ODS |
| Presentations | PPT, PPTX, ODP |
| Emails & Containers | MSG, EML, PST, OST, ZIP |
| Web & Markup | HTML, XHTML, XML |

Full List of Supported [Formats]({{< ref "parser/java/getting-started/supported-document-formats.md" >}})

## Resources

* [API Reference](https://reference.groupdocs.com/parser/java/)
* [Examples on GitHub](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)
* [Licensing](https://purchase.groupdocs.com/policies/use-license/)
* [Free Trial](https://purchase.groupdocs.com/temp-license/100308)
