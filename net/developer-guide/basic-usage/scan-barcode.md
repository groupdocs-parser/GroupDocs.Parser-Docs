---
id: scan-barcode
url: parser/net/scan-barcode
title: Scan Barcode
weight: 5
version: 23.5
description: "Learn how to scan and read barcodes from PDF, Word, Excel, PowerPoint documents and images using GroupDocs.Parser for .NET. Extract barcode values and positions in C# with error handling."
keywords: Scan barcode, code from images, PDF, Emails, Ebooks, Words, barcode scanner C#, read barcode from PDF, barcode extraction
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
tags: csharp, parser, barcode, scanning, v23.5
---
GroupDocs.Parser .Net allows to scan barcode from PDF, Microsoft Office formats: Word (DOC, DOCX), PowerPoint (PPT, PPTX), LibreOffice formats and many others (see full list at [supported document formats]({{< ref "parser/net/getting-started/supported-document-formats.md" >}}) article).

GroupDocs.Parser's barcode scanner is easy to use and powerful at the same time. Our experience allowed us to implement sophisticated algorithms, which allows to read even damaged barcodes.

This article demonstrates how to implement the simplest scenario - read barcode from any supported format without additional settings (to resolve complex scenarios see [advanced usage section]({{< ref "parser/net/developer-guide/advanced-usage/working-with-barcodes/_index.md" >}}). 

## How to scan barcode in .Net with GroupDocs.Parser

To scan barcode from your file, simply call [GetBarcodes](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/methods/getbarcodes) method:

```csharp
IEnumerable<PageBarcodeArea> GetBarcodes();
```

This method returns a collection of [PageBarcodeArea](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea) objects.

Here are the steps to extract a barcode from file:

- Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial file;
- Check if the file supports barcodes extraction;
- Call [GetBarcodes](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/methods/getbarcodes) method and obtain collection of [PageBarcodeArea](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea) objects;
- Iterate through the collection and get a barcode value.

The following example shows how to scan barcodes with error handling:

```csharp
using GroupDocs.Parser;
using GroupDocs.Parser.Data;
using GroupDocs.Parser.Exceptions;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;

try
{
    // Create an instance of Parser class
    using (Parser parser = new Parser(filePath))
    {
        // Check if the file supports barcode extraction
        if (!parser.Features.Barcodes)
        {
            Console.WriteLine("The file doesn't support barcode extraction.");
            return;
        }

        // Scan barcodes from the file
        IEnumerable<PageBarcodeArea> barcodes = parser.GetBarcodes();
        
        if (barcodes == null)
        {
            Console.WriteLine("Barcode extraction returned null.");
            return;
        }
        
        var barcodesList = barcodes.ToList();
        
        if (barcodesList.Count == 0)
        {
            Console.WriteLine("No barcodes found in the document.");
            return;
        }
        
        Console.WriteLine($"Found {barcodesList.Count} barcode(s) in the document.\n");

        // Iterate over barcodes
        int barcodeIndex = 0;
        foreach (PageBarcodeArea barcode in barcodesList)
        {
            barcodeIndex++;
            Console.WriteLine($"=== Barcode {barcodeIndex} ===");
            Console.WriteLine($"Page: {barcode.Page.Index + 1}");
            Console.WriteLine($"Value: {barcode.Value}");
            Console.WriteLine($"Type: {barcode.Type}");
            Console.WriteLine($"Position: {barcode.Rectangle}");
            Console.WriteLine();
        }
    }
}
catch (FileNotFoundException)
{
    Console.WriteLine($"Error: File not found at path '{filePath}'");
}
catch (ParserException ex)
{
    Console.WriteLine($"Error scanning barcodes: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Unexpected error: {ex.Message}");
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