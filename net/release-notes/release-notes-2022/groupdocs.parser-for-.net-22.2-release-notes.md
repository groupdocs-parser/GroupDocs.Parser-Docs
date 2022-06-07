---
id: groupdocs-parser-for-net-22-2-release-notes
url: parser/net/groupdocs-parser-for-net-22-2-release-notes
title: GroupDocs.Parser for .NET 22.2 Release Notes
weight: 2
description: ""
keywords: 
productName: GroupDocs.Parser for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Parser for .NET 22.2{{< /alert >}}

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| PARSERNET-1874 | Implement the support of barcode extraction from images | New Feature |
| PARSERNET-1366 | Implement the ability to parse barcodes in the parse by template functionality | New Feature |
| PARSERNET-1832 | Implement the ability to parse barcodes from documents | New Feature |
| PARSERNET-1830 | Implement the ability to detect images by content | New Feature |
| PARSERNET-1866 | Implement the ability to retrieve the general information about the password-protected file | New Feature |
| PARSERNET-1822 | Improve text area extraction from WordProcessing documents | Improvement |
| PARSERNET-1823 | Improve text area extraction from spreadsheets | Improvement |
| PARSERNET-1868 | Improve the spreadsheet preview functionality | Improvement |
| PARSERNET-1880 | Implement the support for .NET 6 | Improvement |
| PARSERNET-1872 | .NET 6 support | Improvement |

## Public API and Backward Incompatible Changes

### Implement the support of barcode extraction from images 

#### Description

This feature provides the ability to extract barcodes from images.

#### Public API changes

No public API changes.

#### Usage

The following example shows how to extract barcodes from images:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser(Constants.SampleImageWithBarcodes))
{
    // Check if the file supports barcodes extraction
    if (!parser.Features.Barcodes)
    {
        Console.WriteLine("File doesn't support barcodes extraction.");
        return;
    }

    // Extract barcodes from the image.
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

### Implement the ability to parse barcodes in the parse by template functionality

#### Description

This feature provides the ability to extract barcodes from documents.

#### Public API changes

[GroupDocs.Parser.Options.Features](https://apireference.groupdocs.com/parser/net/groupdocs.parser.options/features) public class was updated with changes as follows:

* Added [Barcodes](https://apireference.groupdocs.com/parser/net/groupdocs.parser.options/features/properties/barcodes) property

[PageBarcodeArea](https://apireference.groupdocs.com/parser/net/groupdocs.parser.data/pagebarcodearea) public class was added

[Parser](https://apireference.groupdocs.com/parser/net/groupdocs.parser/parser) public class was updated with changes as follows:

* Added [GetBarcodes](https://apireference.groupdocs.com/parser/net/groupdocs.parser/parser/methods/getbarcodes) method
* Added [GetBarcodes(Int32)](https://apireference.groupdocs.com/parser/net/groupdocs.parser.parser/getbarcodes/methods/2) method
* Added [GetBarcodes(PageAreaOptions)](https://apireference.groupdocs.com/parser/net/groupdocs.parser.parser/getbarcodes/methods/1) method
* Added [GetBarcodes(Int32, PageAreaOptions)](https://apireference.groupdocs.com/parser/net/groupdocs.parser.parser/getbarcodes/methods/3) method

#### Usage

The following example shows how to extract barcodes from a document:

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

    // Extract barcodes from the document.
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

### Implement the ability to detect images by content

#### Description

This feature allows to extract images from email attachments or zip archives in the case when file extension isn't set.

#### Public API changes

No public API changes.

### Usage

The following example shows how to extract all images from the whole document:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser(filePath))
{
    // Extract images
    IEnumerable<PageImageArea> images = parser.GetImages();
    // Check if images extraction is supported
    if (images == null)
    {
        Console.WriteLine("Images extraction isn't supported");
        return;
    }
    // Iterate over images
    foreach (PageImageArea image in images)
    {
        // Print a page index, rectangle and image type:
        Console.WriteLine(string.Format("Page: {0}, R: {1}, Type: {2}", image.Page.Index, image.Rectangle, image.FileType));
    }
}
```

### Implement the ability to parse barcodes from documents

#### Description

This feature allows to define barcode fields in templates.

#### Public API changes

[TemplateBarcode](https://apireference.groupdocs.com/parser/net/groupdocs.parser.templates/templatebarcode) public class was added.

### Usage

The following example shows how to define a template barcode field:

```csharp
// Define a barcode field
TemplateBarcode barcode = new TemplateBarcode(
    new Rectangle(new Point(590, 80), new Size(150, 150)),
    "QR");

// Create a template
Template template = new Template(new TemplateItem[] { barcode });

// Create an instance of Parser class
using (Parser parser = new Parser(Constants.SamplePdfWithBarcodes))
{
    // Parse the document by the template
    DocumentData data = parser.ParseByTemplate(template);

    // Print all extracted data
    for (int i = 0; i < data.Count; i++)
    {
        Console.Write(data[i].Name + ": ");
        PageBarcodeArea area = data[i].PageArea as PageBarcodeArea;
        Console.WriteLine(area == null ? "Not a template barcode field" : area.Value);
    }
}
```

### Implement the ability to retrieve the general information about the password-protected file

#### Description

This feature allows to detect the file type of the password-protected OOXML documents.

#### Public API changes

[Parser](https://apireference.groupdocs.com/parser/net/groupdocs.parser/parser) public class was updated with changes as follows:

* Added [GetFileInfo(Stream, LoadOptions)](https://apireference.groupdocs.com/parser/net/groupdocs.parser.parser/getfileinfo/methods/1) method
* Added [GetFileInfo(String, LoadOptions)](https://apireference.groupdocs.com/parser/net/groupdocs.parser.parser/getfileinfo/methods/3) method

### Usage

The following code shows how to check a file type of the password-protected document:

```csharp
// Get a file info
Options.FileInfo info = Parser.GetFileInfo(filePath, new LoadOptions("password"));
// Check IsEncrypted property
Console.WriteLine(info.IsEncrypted ? "Password is required" : "");
// Print the file type
Console.WriteLine(info.FileType.ToString());
```