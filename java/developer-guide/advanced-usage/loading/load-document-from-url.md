---
id: load-document-from-url
url: parser/java/load-document-from-url
title: Load document from url
weight: 2
description: "Learn how to Load document from url."
keywords: Load document from url, extract data
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---

To avoid the overhead of saving documents to the disk, GroupDocs.Parser enables to extract data from url directly.

The [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) class provides the following constructors that accept a [*java.net.URL*](https://docs.oracle.com/javase/8/docs/api/java/net/URL.html) object:

```java
Parser(java.net.URL url);
Parser(java.net.URL url, LoadOptions loadOptions);
Parser(java.net.URL url, ParserSettings parserSettings);
Parser(java.net.URL url, LoadOptions loadOptions, ParserSettings parserSettings);
```

[LoadOptions](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/LoadOptions) is used to set the file format or the password of the document, and [ParserSettings](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/ParserSettings) is used to customize data extraction (for example, logging or an OCR connector).

The following example shows how to load the document from the url:

```java
URL url = new URL("https://www.bu.edu/csmet/files/2021/03/Getting-Started-with-SQLite.pdf");

// Create an instance of Parser class with the url
try (Parser parser = new Parser(url)) {
    // Extract a text into the reader
    try (TextReader reader = parser.getText()) {
        // Print a text from the document
        // If text extraction isn't supported, a reader is null
        System.out.println(reader == null ? "Text extraction isn't supported" : reader.readToEnd());
    }
}
```

{{< alert style="info" >}}
This constructor makes GroupDocs.Parser download the document itself: the document is read with `URL.openStream()`, so any protocol supported by the Java runtime (including `file:`) is accepted. If the URL comes from untrusted input, validate it (for example, allow only `https` and the expected hosts) before use. See [Network access and data privacy]({{< ref "parser/java/getting-started/network-access-and-data-privacy.md" >}}) for the full list of situations in which the library performs outbound network requests.
{{< /alert >}}

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
