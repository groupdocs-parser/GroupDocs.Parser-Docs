---
id: extract-barcodes-from-document-page
url: parser/java/extract-barcodes-from-document-page
title: Extract barcodes from document page
weight: 2
description: "This article explains that how to extract barcodes from document page."
keywords: extract barcodes 
productName: GroupDocs.Parser for Java
hideChildren: False
---

GroupDocs.Parser provides the functionality to extract barcodes from documents by the [GetBarcodes](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getBarcodes(int)) method:

```java
Iterable<PageBarcodeArea> getBarcodes(int pageIndex)
```

This method returns a collection of [PageBarcodeArea](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea) objects:

| Member | Description |
| --- | --- |
| [Page](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageArea#getPage()) | The page that contains the text area. |
| [Rectangle](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageArea#getRectangle()) | The rectangular area on the page that contains the text area. |
| [Value](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea#getValue()) | A string value that represents a value of the barcode page area. |
| [CodeTypeName](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea#getCodeTypeName()) | A string value than represents a type name of the barcode. |

Here are the steps to extract all barcodes from the whole document:

- Instantiate [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser) object for the initial document;
- Check if the document supports barcodes extraction;
- Call [GetBarcodes](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getBarcodes(int)) method with the page index and obtain collection of [PageBarcodeArea](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea) objects;
- Iterate through the collection and get a barcode value.

The following example shows how to extract barcodes from a document page:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SamplePdfWithBarcodes)) {
	// Check if the document supports barcodes extraction
	if (!parser.getFeatures().isBarcodes()) {
		System.out.println("Document doesn't support barcodes extraction.");
		return;
	}

	// Extract barcodes from the second document page.
	Iterable<PageBarcodeArea> barcodes = parser.getBarcodes(1);

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