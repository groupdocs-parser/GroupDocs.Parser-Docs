---
id: get-supported-features
url: parser/java/get-supported-features
title: Get supported features
weight: 3
description: "This article shows how to check if feature supported for the document."
keywords: Feature,s Parser, Java
productName: GroupDocs.Parser for Java
hideChildren: False
---
The set of the supported features depends on the document format. GroupDocs.Parser provides the functionality to check if feature supported for the document. [getFeatures](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getFeatures()) property is used for this purposes.

[Features](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features) class has the following members:

| Member | Description |
| --- | --- |
| [isFeatureSupported(String)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isFeatureSupported(java.lang.String)) | Returns the value that indicates whether the **feature** is supported. |
| [isText](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isText()) | The value that indicates whether **text extraction** is supported. |
| [isTextPage](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isTextPage()) | The value that indicates whether **text page** extraction is supported. |
| [isFormattedText](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isFormattedText()) | The value that indicates whether **formatted text** extraction is supported. |
| [isFormattedTextPage](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isFormattedTextPage()) | The value that indicates whether **formatted text page** extraction is supported. |
| [isSearch](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isSearch()) | The value that indicates whether **text search** is supported. |
| [isHighlight](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isHighlight()) | The value that indicates whether **highlight extraction** is supported. |
| [isStructure](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isStructure()) | The value that indicates whether **text structure extraction** is supported. |
| [isToc](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isToc()) | The value that indicates whether **table of contents extraction** is supported. |
| [isContainer](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isContainer()) | The value that indicates whether **container extraction** is supported. |
| [isMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isMetadata()) | The value that indicates whether **metadata extraction** is supported. |
| [isTextAreas](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isTextAreas()) | The value that indicates whether **text areas extraction** is supported. |
| [isImages](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isImages()) | The value that indicates whether **images extraction** is supported. |
| [isParseByTemplate](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isParseByTemplate()) | The value that indicates whether **parsing by template** is supported. |
| [isParseForm](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isParseForm()) | The value that indicates whether **form parsing** is supported. |

Here are the steps for check if feature is supported:

*   Instantiate [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object for the initial document;
*   Call corresponding property of [getFeatures](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getFeatures()) property to check if the feature is supported.

The following example shows how to check if text extraction feature is supported:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleZip)) {
    // Check if text extraction is supported for the document
    if (!parser.getFeatures().isText()) {
        System.out.println("Text extraction isn't supported");
        return;
    }
    // Extract a text from the document
    try (TextReader reader = parser.getText()) {
        System.out.println(reader.readToEnd());
    }
}
```

If the feature isn't supported, the method returns *null* instead of the value. So if checking of [Features](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features) properties is omitted, result is checked for *null*:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleZip)) {
    // Extract a text into the reader
    try (TextReader reader = parser.getText()) {
        // Print a text from the document
        // If text extraction isn't supported, a reader is null
        System.out.println(reader == null ? "Text extraction isn't supported" : reader.readToEnd());
    }
}
```

This example prints "Text extraction isn't supported" because there is no text in zip-archive.

Some operations may consume significant time. So it's not optimal to call the method to just check the support for the feature. For this purpose [getFeatures](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getFeatures()) property is used.

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