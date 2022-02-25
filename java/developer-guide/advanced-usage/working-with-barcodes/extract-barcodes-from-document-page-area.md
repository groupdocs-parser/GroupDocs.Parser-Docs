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

GroupDocs.Parser provides the functionality to extract barcodes from documents by the [GetBarcodes](https://apireference-qa.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getBarcodes(com.groupdocs.parser.options.PageAreaOptions)) method:

```java
Iterable<PageBarcodeArea> getBarcodes(PageAreaOptions options)
```

This method returns a collection of [PageBarcodeArea](https://apireference-qa.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea) objects:

| Member | Description |
| --- | --- |
| [Page](https://apireference-qa.groupdocs.com/parser/java/com.groupdocs.parser.data/PageArea#getPage()) | The page that contains the text area.                        |
| [Rectangle](https://apireference-qa.groupdocs.com/parser/java/com.groupdocs.parser.data/PageArea#getRectangle()) | The rectangular area on the page that contains the text area. |
| [Value](https://apireference-qa.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea#getValue()) | A string value that represents a value of the barcode page area. |

Here are the steps to extract all barcodes from the whole document:

- Instantiate [Parser](https://apireference-qa.groupdocs.com/parser/java/com.groupdocs.parser/Parser) object for the initial document;
- Check if the document supports barcodes extraction;
- Instantiate [PageAreaOptions](https://apireference-qa.groupdocs.com/parser/java/com.groupdocs.parser.options/PageAreaOptions) with the rectangular area;
- Call [GetBarcodes](https://apireference-qa.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getBarcodes(com.groupdocs.parser.options.PageAreaOptions)) method and obtain collection of [PageBarcodeArea](https://apireference-qa.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea) objects;
- Iterate through the collection and get a barcode value.

The following example shows how to extract barcodes from the upper-right corner:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SamplePdfWithBarcodes)) {
	// Check if the document supports barcodes extraction
	if (!parser.getFeatures().isBarcodes()) {
		System.out.println("Document doesn't support barcodes extraction.");
		return;
	}

	// Create the options which are used for barcodes extraction
	PageAreaOptions options = new PageAreaOptions(new Rectangle(new Point(590, 80), new Size(150, 150)));
	// Extract barcodes from the upper-right corner.
	Iterable<PageBarcodeArea> barcodes = parser.getBarcodes(options);

	// Iterate over barcodes
	for (PageBarcodeArea barcode : barcodes) {
		// Print the page index
		System.out.println("Page: " + barcode.getPage().getIndex());
		// Print the barcode value
		System.out.println("Value: " + barcode.getValue());
	}
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

- [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)
- [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)

### Free online image extractor App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to extract images from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [GroupDocs Parser App](https://products.groupdocs.app/parser).