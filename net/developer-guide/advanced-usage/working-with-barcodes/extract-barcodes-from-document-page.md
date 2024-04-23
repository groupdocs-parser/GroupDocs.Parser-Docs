---
id: extract-barcodes-from-document-page
url: parser/net/extract-barcodes-from-document-page
title: Extract barcodes from document page
weight: 2
description: "This article explains that how to extract barcodes from document page."
keywords: extract barcodes 
productName: GroupDocs.Parser for .NET
hideChildren: False
---

GroupDocs.Parser provides the functionality to extract barcodes from documents by the [GetBarcodes](https://reference.groupdocs.com/parser/net/groupdocs.parser.parser/getbarcodes/methods/2) method:

```csharp
IEnumerable<PageBarcodeArea> GetBarcodes(int pageIndex);
```

This method returns a collection of [PageBarcodeArea](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea) objects:

| Member | Description |
| --- | --- |
| [Page](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagearea/properties/page) | The page that contains the text area.                        |
| [Rectangle](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagearea/properties/rectangle) | The rectangular area on the page that contains the text area. |
| [Value](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea/properties/value) | A string value that represents a value of the barcode page area. |
| [CodeTypeName](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea/properties/codetypename) | A string value than represents a type name of the barcode. |
| [Confidence](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea/confidence/) | The level of confidence of the parsered barcode. |
| [Angle](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea/angle/) | The angle of the barcode. |

Here are the steps to extract all barcodes from the whole document:

- Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
- Check if the document supports barcodes extraction;
- Call [GetBarcodes](https://reference.groupdocs.com/parser/net/groupdocs.parser.parser/getbarcodes/methods/2) method with the page index and obtain collection of [PageBarcodeArea](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea) objects;
- Iterate through the collection and get a barcode value.

The following example shows how to extract barcodes from a document page:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser(Constants.SamplePdfWithBarcodes))
{
    // Check if the document supports barcodes extraction
    if (!parser.Features.Barcodes)
    {
        Console.WriteLine("Document doesn't support barcodes extraction.");
        return;
    }

    // Extract barcodes from the second document page.
    IEnumerable<PageBarcodeArea> barcodes = parser.GetBarcodes(1);

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

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

- [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)
- [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)

### Free online Barcode Scanner App

Along with full featured .NET library we provide simple, but powerfull free APPs.

You are welcome to scan barcode from your files or mobile phone camera with our free online [GroupDocs Scanner App](https://products.groupdocs.app/scanner/scan-barcode) which is build with GroupDocs.Parser .Net.