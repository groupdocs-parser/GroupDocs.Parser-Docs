---
id: extract-attachments-from-emails
url: parser/java/extract-attachments-from-emails
title: Extract attachments from Emails
weight: 4
description: ""
keywords: 
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---
  

# Extract attachments from Emails

To extract attachments from emails [getContainer](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getContainer()) method is used. This method returns the collection of [ContainerItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem) objects.

Email **Attachment** can contain the following metadata:

| Name | Description |
| --- | --- |
| content-type | The MIME type of the attachment content |
| disposition | The attachment disposition: `inline` or `attachment` |

These metadata refer to a container element itself, not a document.

Here are the steps to extract an email text from email attachments:

*   Instantiate [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object for the initial document;
*   Call [getContainer](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getContainer()) method and obtain collection of [ContainerItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem) objects;
*   Check if *collection* isn't *null* (container extraction is supported for the document);
*   Iterate through the collection and obtain [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object to extract a text.

The following example shows how to extract a text from email attachments:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleMsg)) {
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
        // Print metadata
        for (MetadataItem metadata : item.getMetadata()) {
            System.out.println(String.format("%s: %s", metadata.getName(), metadata.getValue()));
        }
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

## Detect inline images

To detect if image attachment is inline, `disposition` metadata is used. It takes `inline` or `attachment` values. The value is obtained by [getMetadataValue](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem#getMetadataValue(java.lang.String)) method of [ContainerItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem) class.

The following example shows how to detect inline attachments:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.InlineImages)) {
    // Extract attachments from the container
    Iterable<ContainerItem> attachments = parser.getContainer();
    // Check if container extraction is supported
    if (attachments == null) {
        System.out.println("Container extraction isn't supported");
        return;
    }
    // Iterate over attachments
    for (ContainerItem item : attachments) {
        // Check metadata for 'disposition' item
        if ("inline".equals(item.getMetadataValue("disposition"))) {
            // If it's 'inline' then print an item name
            System.out.println(item.getName());
        }
    }
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)  
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).