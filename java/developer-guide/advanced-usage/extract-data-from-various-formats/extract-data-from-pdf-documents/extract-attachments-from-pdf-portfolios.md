---
id: extract-attachments-from-pdf-portfolios
url: parser/java/extract-attachments-from-pdf-portfolios
title: Extract attachments from PDF portfolios
weight: 4
description: ""
keywords: 
productName: GroupDocs.Parser for Java
hideChildren: False
---
  

# Extract attachments from PDF portfolios

To extract attachments from emails [getContainer](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getContainer()) method is used. This method returns the collection of [ContainerItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem) objects.

Here are the steps to extract an attachment text from PDF Portfolios:

*   Instantiate [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object for the initial document;
*   Call [getContainer](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getContainer()) method and obtain collection of [ContainerItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/ContainerItem) objects;
*   Check if *collection* isn't *null* (container extraction is supported for the document);
*   Iterate through the collection and get container item names, sizes and obtain content.

The following example shows how to extract a text from PDF Portfolios:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SamplePdfPortfolio)) {
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

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)
    

### Free online document parser App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).