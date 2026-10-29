---
id: extract-metadata-from-pdf-documents
url: parser/java/extract-metadata-from-pdf-documents
title: Extract metadata from PDF documents
weight: 2
description: "Learn how to extract metadata from PDF files in Java using GroupDocs.Parser for Java. Get document properties such as title, author, subject, creation date, and more."
keywords: extract PDF metadata Java, PDF metadata Java, read PDF document properties Java, get PDF file information, GroupDocs.Parser metadata extraction
productName: GroupDocs.Parser for Java
hideChildren: False
---

PDF files often contain **metadata** such as the title, subject, author, creation date, and application used to generate the document. With **GroupDocs.Parser for Java**, you can easily read these metadata properties programmatically using the [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method.

This guide shows how to extract metadata from PDF documents in Java step by step.

---

## What metadata can be extracted?

The `getMetadata` method can return the following properties from a PDF document:

| Name | Description |
| --- | --- |
| **title** | Title of the PDF document |
| **subject** | Subject of the PDF document |
| **keywords** | Keywords associated with the document |
| **author** | Author of the document |
| **application** | Application that created the PDF |
| **created-time** | Date and time the PDF was created |
| **last-saved-time** | Date and time the PDF was last modified |

*Note: The available metadata depends on the PDF file itself. Properties that are empty in the document are not included in the result.*

---

## How to extract PDF metadata in Java

Follow these steps to get metadata from a PDF document:

1. **Instantiate the [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object** for the PDF file.
2. **Call the [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method** to retrieve the collection of [MetadataItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/MetadataItem) objects.
3. **Iterate through the collection** and read metadata names and values.

### Example: extract PDF metadata in Java

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SamplePdf)) {
    // Extract metadata from the document
    Iterable<MetadataItem> metadata = parser.getMetadata();
    // Iterate over metadata items
    for (MetadataItem item : metadata) {
        // Print an item name and value
        System.out.println(String.format("%s: %s", item.getName(), item.getValue()));
    }
}
```

{{< alert style="warning" >}}
[getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method returns *null* value if metadata extraction isn't supported for the document. For example, metadata extraction isn't supported for TXT files. Therefore, for TXT file [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata())  method returns *null*. If PDF document has no metadata, [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method returns an empty collection.
{{< /alert >}}

## Why extract PDF metadata?

Extracting PDF metadata is useful for:

*   Document management – quickly find and categorize files by author, subject, or keywords.
*   Compliance checks – verify creation and modification dates.
*   Search indexing – improve search accuracy by including document properties.

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
