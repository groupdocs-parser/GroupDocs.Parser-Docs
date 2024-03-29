---
id: iterate-through-container-items
url: parser/java/iterate-through-container-items
title: Iterate through container items
weight: 1
description: "This article explains that how to extract containers items and iterate through container items in Java."
keywords: extract containers items, iterate through container items
productName: GroupDocs.Parser for Java
hideChildren: False
---
  
GroupDocs.Parser provides the functionality to extract items from containers by the [getContainer](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getContainer()) method:

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
| [openStream](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem#openStream()) | Opens the stream of the item content. |
| [openParser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem#openParser()) | Creates the Parser object for the item content. |
| [openParser(LoadOptions)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem#openParser(com.groupdocs.parser.options.LoadOptions)) | Creates the Parser object for the item content with [LoadOptions](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/LoadOptions). |
| [openParser(LoadOptions, ParserSettings)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem#openParser(com.groupdocs.parser.options.LoadOptions,%20com.groupdocs.parser.options.ParserSettings)) | Creates the Parser object for the item content with [LoadOptions](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/LoadOptions) and [ParserSettings](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/ParserSettings). |

Here are the steps to extract container from the document:

*   Instantiate [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object for the initial document;
*   Call [getContainer](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getContainer()) method and obtain collection of document [ContainerItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem) objects;
*   Check if *collection* isn't *null* (container extraction is supported for the document);
*   Iterate through the collection and get container item names, sizes and obtain content.

The following example shows how to extract attachments from a container:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleZip)) {
    // Extract attachments from the container
    Iterable<ContainerItem> attachments = parser.getContainer();
    // Check if container extraction is supported
    if (attachments == null) {
        System.out.println("Container extraction isn't supported");
    }
    // Iterate over attachments
    for (ContainerItem item : attachments) {
        // Print an item name and size
        System.out.println(String.format("%s: &s", item.getName(), item.getSize()));
    }
}
```

Container represents both container-only files (like zip archives, outlook storage) and documents with attachments (like emails, PDF Portfolios).

In case of outlook storage (ost/pst files) container consists of email documents (msg files).

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).