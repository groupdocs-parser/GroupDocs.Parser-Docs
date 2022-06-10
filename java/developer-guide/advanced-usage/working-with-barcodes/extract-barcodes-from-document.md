---
id: extract-barcodes-from-document
url: parser/java/extract-barcodes-from-document
title: Extract barcodes from document
weight: 1
description: "This article explains that how to extract barcodes from documents."
keywords: extract barcodes
productName: GroupDocs.Parser for Java
hideChildren: False
---

GroupDocs.Parser provides the functionality to extract barcodes from documents by the [GetBarcodes](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getBarcodes()) method:

```java
Iterable<PageBarcodeArea> getBarcodes()
```

This method returns a collection of [PageBarcodeArea](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea) objects:

| Member | Description |
| --- | --- |
| [Page](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageArea#getPage()) | The page that contains the text area.                        |
| [Rectangle](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageArea#getRectangle()) | The rectangular area on the page that contains the text area. |
| [Value](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea#getValue()) | A string value that represents a value of the barcode page area. |
| [CodeTypeName](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea#getCodeTypeName()) | A string value than represents a type name of the barcode. |

Here are the steps to extract all barcodes from the whole document:

- Instantiate [Parser](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser/Parser) object for the initial document;
- Check if the document supports barcodes extraction;
- Call [GetBarcodes](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getBarcodes()) method and obtain collection of [PageBarcodeArea](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea) objects;
- Iterate through the collection and get a barcode value.

The following example shows how to extract barcodes from a document:

```java
// Create an instance of Parser class
try(Parser parser = new Parser(Constants.SamplePdfWithBarcodes))
{
	// Check if the document supports barcodes extraction
	if (!parser.getFeatures().isBarcodes()) {
		System.out.println("Document doesn't support barcodes extraction.");
		return;
	}

	// Extract barcodes from the document.
	Iterable<PageBarcodeArea> barcodes = parser.getBarcodes();

	// Iterate over barcodes
	for(PageBarcodeArea barcode : barcodes)
	{
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