---
id: extract-data-from-attachments-and-zip-archives
url: parser/java/extract-data-from-attachments-and-zip-archives
title: Extract data from attachments and ZIP archives
weight: 9
description: "This article shows how to extract data (text, images, PDF forms) from ZIP-archived documents with GroupDocs.Parser."
keywords: Extract data, PDF forms, ZIP-archived documents
productName: GroupDocs.Parser for Java
hideChildren: False
---
It is easy to extract data, text, images and use any GroupDocs.Parser feature for ZIP-archived documents. The same feature allows to get attachments from PDF and Emails and extract data from them.

# Extract data from attachments and ZIP archives

To extract documents from ZIP files and get attachments from containers simply call the [getContainer](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getContainer()) method:

```java
Iterable<ContainerItem> getContainer();
```

This method returns a collection of [ContainerItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem) objects:

| Member | Description |
| --- | --- |
| [getName](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem#getName()) | The name of the item. |
| [getDirectory](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem#getDirectory()) | The directory of the item. |
| [getFilePath](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem#getFilePath()) | The full path of the item. |
| [getSize](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem#getSize()) | The size of the item in bytes. |
| [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem#getMetadata()) | The collection of item metadata. |
| [detectFileType(FileTypeDetectionMode)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/ContainerItem#detectFileType(com.groupdocs.parser.options.FileTypeDetectionMode)) | Detects a file type of the container item. |
| [openStream](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem#openStream()) | Opens the stream of the item content. |
| [openParser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem#openParser()) | Creates the Parser object for the item content. |
| [openParser(LoadOptions)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem#openParser(com.groupdocs.parser.options.LoadOptions)) | Creates the Parser object for the item content with [LoadOptions](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/LoadOptions). |
| [openParser(LoadOptions, ParserSettings)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem#openParser(com.groupdocs.parser.options.LoadOptions,%20com.groupdocs.parser.options.ParserSettings)) | Creates the Parser object for the item content with [LoadOptions](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/LoadOptions) and [ParserSettings](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/ParserSettings). |

Container represents both container-only files (like zip archives, outlook storage) and documents with attachments (like emails, PDF Portfolios).

Here are the steps to extract a text from from zip entities:

*   Instantiate [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object for the initial document;
*   Call [getContainer](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getContainer()) method and obtain collection of document container item objects;
*   Check if *collection* isn't *null* (container extraction is supported for the document);
*   Iterate through the collection and obtain [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object to extract a text.

The following example shows how to extract a text from from zip entities:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleZip)) {
    // Extract attachments from the container
    Iterable<ContainerItem> attachments = parser.getContainer();
    // Check if container extraction is supported
    if (attachments == null) {
        System.out.println("Container extraction isn't supported");
    }
    // Iterate over zip entities
    for (ContainerItem item : attachments) {
        // Print the file path
        System.out.println(item.getFilePath());
        try {
            // Create Parser object for the zip entity content
            try (Parser attachmentParser = item.openParser()) {
                // Extract an zip entity text
                try (TextReader reader = attachmentParser.getText()) {
                    System.out.println(reader == null ? "No text" : reader.readToEnd());
                }
            }
        } catch (UnsupportedDocumentFormatException ex) {
            System.out.println("Isn't supported.");
        }
    }
}
```

## More resources

### Advanced usage topics

To learn more about document data extraction features and get familiar how to extract text, images, forms and more, please refer to the [advanced usage section]({{< ref "parser/java/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).