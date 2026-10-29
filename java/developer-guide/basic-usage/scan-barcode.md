---
id: scan-barcode
url: parser/java/scan-barcode
title: Scan Barcode
weight: 5
description: "Learn how to scan and read barcodes from PDF, Word, Excel, PowerPoint documents and images using GroupDocs.Parser for Java. Extract barcode values and positions in Java."
keywords: Scan barcode, code from images, PDF, Emails, Ebooks, Words, barcode scanner Java, read barcode from PDF, barcode extraction
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---
GroupDocs.Parser for Java allows to scan barcodes from PDF, Microsoft Office formats: Word (DOC, DOCX), PowerPoint (PPT, PPTX), LibreOffice formats, images and many others (see full list at [supported document formats]({{< ref "parser/java/getting-started/supported-document-formats.md" >}}) article).

GroupDocs.Parser's barcode scanner is easy to use and powerful at the same time. Its algorithms allow to read even damaged barcodes.

This article demonstrates how to implement the simplest scenario - read barcodes from any supported format without additional settings (to resolve complex scenarios see [advanced usage section]({{< ref "parser/java/developer-guide/advanced-usage/working-with-barcodes/_index.md" >}})).

## How to scan barcode in Java with GroupDocs.Parser

To scan barcodes from your file, simply call [getBarcodes](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getBarcodes()) method:

```java
Iterable<PageBarcodeArea> getBarcodes()
```

This method returns a collection of [PageBarcodeArea](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea) objects.

Here are the steps to extract barcodes from a file:

- Instantiate [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser) object for the initial file;
- Check if the file supports barcodes extraction;
- Call [getBarcodes](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getBarcodes()) method and obtain collection of [PageBarcodeArea](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea) objects;
- Iterate through the collection and get a barcode value.

The following example shows how to scan barcodes:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SamplePdfWithBarcodes)) {
    // Check if the file supports barcodes extraction
    if (!parser.getFeatures().isBarcodes()) {
        System.out.println("The file doesn't support barcodes extraction.");
        return;
    }

    // Scan barcodes from the file
    Iterable<PageBarcodeArea> barcodes = parser.getBarcodes();

    // Iterate over barcodes
    for (PageBarcodeArea barcode : barcodes) {
        // Print the page index
        System.out.println("Page: " + barcode.getPage().getIndex());
        // Print the barcode type and value
        System.out.println(barcode.getCodeTypeName() + ": " + barcode.getValue());
    }
}
```

## More resources

### Advanced usage topics

To learn more about scan barcode features, please refer the [advanced usage section]({{< ref "parser/java/developer-guide/advanced-usage/working-with-barcodes/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online Barcode Scanner App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to scan barcode from your files or mobile phone camera with our free online [GroupDocs Scanner App](https://products.groupdocs.app/scanner/scan-barcode) which is built with GroupDocs.Parser.
