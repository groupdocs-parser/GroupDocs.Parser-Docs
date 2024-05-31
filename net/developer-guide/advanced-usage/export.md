---
id: export-data
url: parser/net/export-data
title: Export Data
weight: 106
description: "How to export data to JSON or XML files."
keywords: json xml export
productName: GroupDocs.Parser for .NET
hideChildren: False
---
GroupDocs.Parser provides the functionality to export data to JSON or XML files. [JsonExporter](https://reference.groupdocs.com/parser/net/groupdocs.parser.export/jsonexporter) or [XmlExporter](https://reference.groupdocs.com/parser/net/groupdocs.parser.export/xmlexporter) classes are used for this. Also extensions methods from [Extensions](https://reference.groupdocs.com/parser/net/groupdocs.parser.export/extensions) class can be used to simplify the code.

The following example shows how to extract barcodes from a document and export them to the JSON file:

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
    BarcodeOptions options = new BarcodeOptions(QualityMode.Low, QualityMode.Low, "QR");

    // Extract barcodes from the document
    IEnumerable<PageBarcodeArea> barcodes = parser.GetBarcodes(options);
    
    // Export data to "data.json" file
    barcodes.ExportAsJson("data.json");
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