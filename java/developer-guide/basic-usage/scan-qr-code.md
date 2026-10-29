---
id: scan-qr-code
url: parser/java/scan-qr-code
title: Scan QR Code
weight: 5
description: "This article shows how to scan QR Code in Java with GroupDocs.Parser with few lines of code from image, document or other file format like PDF, Email, Ebook, Words, and others."
keywords: Scan QR Code, PDF, Email, Ebook, Words, Java
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---
GroupDocs.Parser for Java allows to scan QR Codes from PDF, MS Word (DOC, DOCX), MS PowerPoint (PPT, PPTX), LibreOffice formats, images and many others (see full list at [supported document formats]({{< ref "parser/java/getting-started/supported-document-formats.md" >}}) article).

GroupDocs.Parser's QR Code scanner is easy to use and powerful at the same time. Its algorithms allow to read even damaged barcodes.

This article demonstrates how to implement QR Code reader for any supported format.

## How to scan QR Code in Java with GroupDocs.Parser

To scan QR Codes from your file, call [getBarcodes(BarcodeOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getBarcodes(com.groupdocs.parser.options.BarcodeOptions)) method with [BarcodeOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/BarcodeOptions) that restrict the barcode types to `"QR"`:

```java
Iterable<PageBarcodeArea> getBarcodes(BarcodeOptions options)
```

This method returns a collection of [PageBarcodeArea](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea) objects.

Here are the steps to extract QR Codes from a file:

- Instantiate [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser) object for the initial file;
- Check if the file supports barcodes extraction;
- Instantiate [BarcodeOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/BarcodeOptions) object with `"QR"` code type;
- Call [getBarcodes(BarcodeOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getBarcodes(com.groupdocs.parser.options.BarcodeOptions)) method and obtain collection of [PageBarcodeArea](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea) objects;
- Iterate through the collection and get a QR Code value.

The following example shows how to scan QR Codes:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SamplePdfWithBarcodes)) {
    // Check if the file supports barcodes extraction
    if (!parser.getFeatures().isBarcodes()) {
        System.out.println("The file doesn't support barcodes extraction.");
        return;
    }

    // Create the options to scan barcodes with "QR" type only
    BarcodeOptions options = new BarcodeOptions(QualityMode.Low, QualityMode.Low, "QR");

    // Scan QR Codes from the file
    Iterable<PageBarcodeArea> qrcodes = parser.getBarcodes(options);

    // Iterate over QR Codes
    for (PageBarcodeArea qrcode : qrcodes) {
        // Print the page index
        System.out.println("Page: " + qrcode.getPage().getIndex());
        // Print the QR Code value
        System.out.println("Value: " + qrcode.getValue());
    }
}
```

Alternatively, scan all barcodes with [getBarcodes()](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getBarcodes()) and keep the ones whose [getCodeTypeName](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea#getCodeTypeName()) method returns `"QR"`:

```java
// Iterate over all barcodes
for (PageBarcodeArea barcode : parser.getBarcodes()) {
    // Skip barcodes of other types
    if (!"QR".equals(barcode.getCodeTypeName())) {
        continue;
    }
    // Print the QR Code value
    System.out.println("Value: " + barcode.getValue());
}
```

## More resources

### Advanced usage topics

To learn more about scan barcode features, please refer the [advanced usage section]({{< ref "parser/java/developer-guide/advanced-usage/working-with-barcodes/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online QR Code Scanner App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to scan QR Code from your file with our free online [GroupDocs Scanner App](https://products.groupdocs.app/scanner/scan-qr-code) which is built with GroupDocs.Parser.
