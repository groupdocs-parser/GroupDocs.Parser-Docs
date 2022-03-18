---
id: groupdocs-parser-for-java-22-3-release-notes
url: parser/java/groupdocs-parser-for-java-22-3-release-notes
title: GroupDocs.Parser for Java 22.3 Release Notes
weight: 1
description: ""
keywords: 
productName: GroupDocs.Parser for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Parser for Java 22.3{{< /alert >}}

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| PARSERNET-1874 | Implement the support of barcode extraction from images | New Feature |
| PARSERNET-1366 | Implement the ability to parse barcodes in the parse by template functionality | New Feature |
| PARSERNET-1832 | Implement the ability to parse barcodes from documents | New Feature |
| PARSERNET-1830 | Implement the ability to detect images by content | New Feature |
| PARSERNET-1822 | Improve text area extraction from WordProcessing documents | Improvement |
| PARSERNET-1823 | Improve text area extraction from spreadsheets | Improvement |
| PARSERNET-1868 | Improve the spreadsheet preview functionality | Improvement |
| PARSERNET-1776 | Implement Text property for FieldData class | Improvement |

## Public API and Backward Incompatible Changes

### Implement the support of barcode extraction from images

#### Description

This feature provides the ability to extract barcodes from images.

#### Public API changes

No public API changes.

#### Usage

The following example shows how to extract barcodes from images:

```java
// Create an instance of Parser class
try(Parser parser = new Parser(Constants.SamplePdfWithBarcodes))
{
	// Check if the file supports barcodes extraction
	if (!parser.getFeatures().isBarcodes()) {
		System.out.println("File doesn't support barcodes extraction.");
		return;
	}

	// Extract barcodes from the image.
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

### Implement the ability to parse barcodes in the parse by template functionality

#### Description

This feature provides the ability to extract barcodes from documents.

#### Public API changes

[GroupDocs.Parser.Options.Features](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser.options/Features) public class was updated with changes as follows:

* Added [Barcodes](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser.options/Features#isBarcodes()) property

[PageBarcodeArea](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser.data/PageBarcodeArea) public class was added

[Parser](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser/Parser) public class was updated with changes as follows:

* Added [getBarcodes](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getBarcodes()) method
* Added [getBarcodes(int)](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getBarcodes(int)) method
* Added [getBarcodes(PageAreaOptions)](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getBarcodes(com.groupdocs.parser.options.PageAreaOptions)) method
* Added [getBarcodes(int, PageAreaOptions)](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#getBarcodes(int,%20com.groupdocs.parser.options.PageAreaOptions)) method

#### Usage

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

### Implement the ability to detect images by content

#### Description

This feature allows to extract images from email attachments or zip archives in the case when file extension isn't set.

#### Public API changes

No public API changes.

### Usage

The following example shows how to extract all images from the whole document:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleImagesPdf)) {
    // Extract images
    Iterable<PageImageArea> images = parser.getImages();
    // Check if images extraction is supported
    if (images == null) {
        System.out.println("Images extraction isn't supported");
        return;
    }
    // Iterate over images
    for (PageImageArea image : images) {
        // Print a page index, rectangle and image type:
        System.out.println(String.format("Page: %d, R: %s, Type: %s", image.getPage().getIndex(), image.getRectangle(), image.getFileType()));
    }
}
```

### Implement the ability to parse barcodes from documents

#### Description

This feature allows to define barcode fields in templates.

#### Public API changes

[TemplateBarcode](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser.templates/TemplateBarcode) public class was added.

### Usage

The following example shows how to define a template barcode field:

```java
// Define a barcode field
TemplateBarcode barcode = new TemplateBarcode(
		new Rectangle(new Point(590, 80), new Size(150, 150)),
		"QR");

// Create a template
Template template = new Template(Arrays.asList(new TemplateItem[]{barcode}));

// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SamplePdfWithBarcodes)) {
	// Parse the document by the template
	DocumentData data = parser.parseByTemplate(template);

	// Print all extracted data
	for (int i = 0; i < data.getCount(); i++) {
		// Print field name
		System.out.print(data.get(i).getName() + ": ");

		// As we have defined only barcode fields in the template,
		// we cast PageArea property value to PageBarcodeArea
		PageBarcodeArea area = data.get(i).getPageArea() instanceof PageBarcodeArea
				? (PageBarcodeArea) data.get(i).getPageArea()
				: null;
		System.out.println(area == null ? "Not a template barcode field" : area.getValue());
	}
}
```

### Implement Text property for FieldData class

#### Description

This improvement enhanced the work with text fields.

#### Public API changes

[FieldData](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser.data/FieldData) public class was updated with changes as follows:

* Added [Text](https://apireference.groupdocs.com/parser/java/com.groupdocs.parser.data/FieldData#getText()) property

#### Usage

The following example how to extract data from text fields:

```java
// Get the field data
FieldData field = data.get(i);
// Check if the field data contains a text
if(field.getText() != null)
{
    // Print the field text value
    System.out.println(field.getText());
}
```