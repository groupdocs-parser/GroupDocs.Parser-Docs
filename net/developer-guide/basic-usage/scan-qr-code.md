---
id: scan-qr-code
url: parser/net/scan-qr-code
title: Scan QR Code
weight: 5
description: " This article shows how to scan QR Code in .Net with GroupDocs.Parser with few lines of code from image, document or other file format like PDF, Email, Ebook, Words, and others."
keywords: Scan QR Code, PDF, Email, Ebook, Words
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---
GroupDocs.Parser .Net allows to scan QR Code from PDF, MS Word (DOC, DOCX), MS PowerPoint (PPT, PPTX), LibreOffice formats and many others (see full list at [supported document formats]({{< ref "parser/net/getting-started/supported-document-formats.md" >}}) article).

GroupDocs.Parser's QR Code scanner is easy to use and powerful at the same time. Our sophisticated algorithms allow to read even damaged barcodes.

This article demonstrates how to implement QR Code reader from any supported format. 

## How to scan QR Code in .Net with GroupDocs.Parser

To scan QR Code from your file, simply call [GetBarcodes](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/methods/getbarcodes) method:

```csharp
IEnumerable<PageBarcodeArea> GetBarcodes();
```

This method returns a collection of [PageBarcodeArea](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea) objects.

Here are the steps to extract a QR Code from file:

- Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial file;
- Check if the file supports barcodes extraction;
- Call [GetBarcodes](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/methods/getbarcodes) method and obtain collection of [PageBarcodeArea](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea) objects;
- Iterate through the collection and get a barcode value.

The following example shows how to scan barcode:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser(Constants.SamplePdfWithBarcodes))
{
    // Check if the file supports barcodes extraction
    if (!parser.Features.Barcodes)
    {
        Console.WriteLine("The file doesn't support barcodes extraction.");
        return;
    }

    // Scan barcodes with type "QR" from your file.
    IEnumerable<PageBarcodeArea> qrcodes = parser.GetBarcodes().Where(i => i.CodeTypeName == "QR");

    // Iterate over barcodes
    foreach (PageBarcodeArea qrcode in qrcodes)
    {
        // Print the page index
        Console.WriteLine("Page: " + qrcode.Page.Index.ToString());
        // Print the qrcode value
        Console.WriteLine("Value: " + qrcode.Value);
    }
}
```

## More resources

### Advanced usage topics

To learn more about scan barcode features, please refer the [advanced usage section]({{< ref "parser/net/developer-guide/advanced-usage/working-with-barcodes/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online text extractor App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to scan QR Code from your file with our free online [GroupDocs Scanner App](https://products.groupdocs.app/scanner/scan-qr-code) which is build with GroupDocs.Parser .Net.