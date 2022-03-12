---
id: scan-barcode
url: parser/net/scan-barcode
title: Scan Barcode
weight: 5
description: "Scan barcode in .Net with GroupDocs.Parser with few lines of code from images, documents and other file formats like PDF, Emails, Ebooks, Word, and others."
keywords: 
productName: GroupDocs.Parser for .NET
hideChildren: False
---
GroupDocs.Parser .Net allows to scan barcode from PDF, Microsoft Office formats: Word (DOC, DOCX), PowerPoint (PPT, PPTX), LibreOffice formats and many others (see full list at [supported document formats]({{< ref "parser/net/getting-started/supported-document-formats.md" >}}) article).

GroupDocs.Parser's barcode scanner is easy to use and powerful at the same time. Our experience allowed us to implement sophisticated algorithms, which allows to read even damaged barcodes.

This article demonstrates how to implement the simplest scenario - read barcode from any supported format without additional settings (to resolve complex scenarios see [advanced usage section]({{< ref "parser/net/developer-guide/advanced-usage/working-with-barcodes/_index.md" >}}). 

## How to scan barcode in .Net with GroupDocs.Parser

To scan barcode from your file, simply call [GetBarcodes](https://apireference.groupdocs.com/parser/net/groupdocs.parser/parser/methods/getbarcodes) method:

```csharp
IEnumerable<PageBarcodeArea> GetBarcodes();
```

This method returns a collection of [PageBarcodeArea](https://apireference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea) objects.

Here are the steps to extract a barcode from file:

- Instantiate [Parser](https://apireference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial file;
- Check if the file supports barcodes extraction;
- Call [GetBarcodes](https://apireference.groupdocs.com/parser/net/groupdocs.parser/parser/methods/getbarcodes) method and obtain collection of [PageBarcodeArea](https://apireference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea) objects;
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

    // Scan barcodes from the file.
    IEnumerable<PageBarcodeArea> barcodes = parser.GetBarcodes();

    // Iterate over barcodes
    foreach (PageBarcodeArea barcode in barcodes)
    {
        // Print the page index
        Console.WriteLine("Page: " + barcode.Page.Index.ToString());
        // Print the barcode value
        Console.WriteLine("Value: " + barcode.Value);
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

### Free online Barcode Scanner App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to scan barcode from your files or mobile phone camera with our free online [GroupDocs Scanner App](https://products.groupdocs.app/scanner/scan-barcode) which is build with GroupDocs.Parser .Net.