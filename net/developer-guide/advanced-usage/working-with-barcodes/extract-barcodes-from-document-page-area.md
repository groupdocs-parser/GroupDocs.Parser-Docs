---
id: extract-barcodes-from-document-page-area
url: parser/net/extract-barcodes-from-document-page-area
title: Extract barcodes from document page area
weight: 3
description: ""
keywords: 
productName: GroupDocs.Parser for .NET
hideChildren: False
---

GroupDocs.Parser provides the functionality to extract barcodes from documents by the [GetBarcodes](https://apireference.groupdocs.com/parser/net/groupdocs.parser.parser/getbarcodes/methods/1) method:

```csharp
IEnumerable<PageBarcodeArea> GetBarcodes(PageAreaOptions options);
```

This method returns a collection of [PageBarcodeArea](https://apireference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea) objects:

| Member | Description |
| --- | --- |
| [Page](https://apireference.groupdocs.com/net/parser/groupdocs.parser.data/pagearea/properties/page) | The page that contains the text area.                        |
| [Rectangle](https://apireference.groupdocs.com/net/parser/groupdocs.parser.data/pagearea/properties/rectangle) | The rectangular area on the page that contains the text area. |
| [Value](https://apireference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea/properties/value) | A string value that represents a value of the barcode page area. |
| [CodeTypeName](https://apireference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea/properties/codetypename) | A string value than represents a type name of the barcode. |

Here are the steps to extract all barcodes from the whole document:

- Instantiate [Parser](https://apireference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
- Check if the document supports barcodes extraction;
- Instantiate [PageAreaOptions](https://apireference.groupdocs.com/net/parser/groupdocs.parser.options/pageareaoptions) with the rectangular area;
- Call [GetBarcodes](https://apireference.groupdocs.com/parser/net/groupdocs.parser.parser/getbarcodes/methods/1) method and obtain collection of [PageBarcodeArea](https://apireference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea) objects;
- Iterate through the collection and get a barcode value.

The following example shows how to extract barcodes from the upper-right corner:

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

    // Create the options which are used for barcodes extraction
    PageAreaOptions options = new PageAreaOptions(new Rectangle(new Point(590, 80), new Size(150, 150)));
    // Extract barcodes from the upper-right corner.
    IEnumerable<PageBarcodeArea> barcodes = parser.GetBarcodes(options);

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

### Free online image extractor App

Along with full featured .NET library we provide simple, but powerfull free APPs.

You are welcome to extract images from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [GroupDocs Parser App](https://products.groupdocs.app/parser).