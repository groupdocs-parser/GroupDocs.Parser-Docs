---
id: working-with-templates
url: parser/java/working-with-templates
title: Working with templates
weight: 101
description: ""
keywords: 
productName: GroupDocs.Parser for Java
hideChildren: False
---
Document template is set by [Template](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/Template) class. It contains template items - fields and tables. Each item has the unique (in the template bounds) name and optional page index - value that represents the index of the page where the template item is located; *null* if the template item is located on any page.

## Template fields

The template field is set by [TemplateField](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateField) class with the following constructor:

```java
TemplateField(TemplatePosition position, String name, Integer pageIndex)

```

| Parameter | Description |
| --- | --- |
| position | Defines the way how to find the field on a page. |
| name | A unique template item name. |
| pageIndex | The page index. An integer value that represents the index of the page where the template item is located; null if the template item is located on any page. |

[TemplatePosition](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplatePosition) is an abstract base class. The following classes are used to set template positions:

*   [TemplateFixedPosition](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateFixedPosition). Provides a template field position which is defined by the rectangular area.
*   [TemplateRegexPosition](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateRegexPosition). Provides a template field position which uses the regular expression.
*   [TemplateLinkedPosition](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateLinkedPosition). Provides a template field position which uses the linked field.

### TemplateFixedPosition

This is simplest way to define the field position. It requires to set a rectangular area on the page that bounds the field value. All the text that is contained (even partially) into the rectangular area will be extracted as a value:

```java
// Create a fixed template field with "Address" name which is bounded by a rectangle at the position (35, 160) and with the size (110, 20)
TemplateField templateField = new TemplateField(
    new TemplateFixedPosition(new Rectangle(new Point(35, 160), new Size(110, 20))),
    "Address");

```

It is recommended to define a rectangular area above (below) the center of the line that is below (above) the selected area, in order to avoid the excessive extraction of the text. For example:

| Template definition | Result |
| --- | --- |
| ![](/parser/java/images/working-with-templates.png)
  | Extracts only one line:  
67890 |
| ![](/parser/java/images/working-with-templates_1.png)) | Extracts two lines
4321 First Street  
Anytown, State ZIP |
|  ![](/parser/java/images/working-with-templates_2.png)) | Extracts four lines
Company Name  
4321 First Street  
Anytown, State ZIP  
Date: 06/02/2019 |

### TemplateRegexPosition

This way to define the field position allows to find a field value by a regular expression. For example, if the document contains "Invoice Number INV-12345" then template field can be defined in the following way:

```java
// Create a regex template field with "InvoiceNumber" name
TemplateField templateField = new TemplateField(
    new TemplateRegexPosition("Invoice Number\\s+[A-Z0-9\\-]+"), 
    "InvoiceNumber");

```

In this case as a value the entire string is extracted. To extract only a part of the string the regular expression group "value" is used:

```java
// Create a regex template field with "InvoiceNumber" name with "value" group
TemplateField templateField = new TemplateField(
    new TemplateRegexPosition("Invoice Number\\s+(?<value>[A-Z0-9\\-]+)"),
    "InvoiceNumber");

```

In this case as a value "INV-3337" string is extracted.

Regular expression fields can be used as linked fields.

### TemplateLinkedPosition

This way to define the field position allows to find a field value by extracting a rectangular area around the linked field. For example, if it's known that the field with an invoice number is placed on the right of "Invoice number" string the following code is used:

```java
// Create a regex template field to find "Invoice Number" text
TemplateField invoice = new TemplateField(new TemplateRegexPosition("Invoice Number"), "Invoice");
// Create a related template field associated with "Invoice" field and extract the value on the right of it
TemplateField invoiceNumber = new TemplateField(
    new TemplateLinkedPosition("invoice", new Size(100, 15), new TemplateLinkedPositionEdges(false, false, true, false)),
    "InvoiceNumber");

```

| Template definition | Result |
| --- | --- |
| ![](/parser/java/images/working-with-templates_3.png)) | Extracts a text on the right of "Invoice Number" field:INV-3337 

To simplify the setting of the size of template field [isAutoScale](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateLinkedPosition#isAutoScale()) property is used. The size of template field is scaled according to the related field if [isAutoScale](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateLinkedPosition#isAutoScale()) is set to true. This is useful when the font size is not known in advance, but the proportions of the size of the value (the ratio of height to width) are approximately known:

```java
// Create a regex template field to find "Invoice Number" text
TemplateField invoice = new TemplateField(new TemplateRegexPosition("Invoice Number"), "Invoice");
// Create a related template field associated with "Invoice" field and extract the value on the right of it
TemplateField invoiceNumber = new TemplateField(
    new TemplateLinkedPosition("invoice", new Size(100, 15), new TemplateLinkedPositionEdges(false, false, true, false), true),
    "InvoiceNumber");

```

| Template definition | Result |
| --- | --- |
| ![](/parser/java/images/working-with-templates_4.png)) | Extracts a text on the right of "Invoice Number" field:INV-3337 

The field value can be extracted from either side of the related field. The side of the value extraction is set by [getEdges](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateLinkedPosition#getEdges()) property. The size of rectangular area is set by [getSearchArea](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateLinkedPosition#getSearchArea()) property. The position of rectangular area depends on the side of the value extraction:

```java
Left: (LinkedField.Rectangle.Left - SearchAreaSize.Width; LinkedField.Rectangle.Top)
Top: (LinkedField.Rectangle.Left; LinkedField.Rectangle.Top - SearchAreaSize.Height)
Right: (LinkedField.Rectangle.Right; LinkedField.Rectangle.Top)
Bottom: (LinkedField.Rectangle.Left; LinkedField.Rectangle.Bottom)

```

The related field can be any field which was previously defined in the template:

```java
// Create a regex template field
TemplateField fromField = new TemplateField(new TemplateRegexPosition("From"), "From", 0);
// Create a related template field linked to "From" regex field and placed under it
TemplateField companyField = new TemplateField(
    new TemplateLinkedPosition("From", new Size(100, 10), new TemplateLinkedPositionEdges(false, false, false, true)),
    "FromCompany", 
    0);
// Create a related template field linked to "FromCompany" related field and placed under it
TemplateField addressField = new TemplateField(
    new TemplateLinkedPosition("FromCompany", new Size(100, 30), new TemplateLinkedPositionEdges(false, false, false, true)),
    "FromAddress", 
    0);

```

| Template definition | Result |
| --- | --- |
| ![](/parser/java/images/working-with-templates_5.png)) | The extraction is processed in the following way:Extracts data of "From" regex field (green
Extracts data of "FromCompany" related field (yellow)  
Extracts data of "FromAddress" related field (red) |

A value of the field depends on the related field. The field is always empty if the related field doesn't have a value. If the field has a value then it has a link to the related field.

### Document template with fields

An instance of [Template](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/Template) class is created by the constructor:

```java
Template(IEnumerable<TemplateItem> items)

```

This constructor accepts a collection of template items:

```java
// Create an array of template fields
TemplateItem[] fields = new TemplateItem[]
{
    new TemplateField(new TemplateRegexPosition("From"), "From", 0),
    new TemplateField(
        new TemplateLinkedPosition("From", new Size(100, 10), new TemplateLinkedPositionEdges(false, false, false, true)),
        "FromCompany",
        0),
    new TemplateField(
        new TemplateLinkedPosition("FromCompany", new Size(100, 30), new TemplateLinkedPositionEdges(false, false, false, true)),
        "FromAddress",
        0)
};
// Create a document template
Template template = new Template(java.util.Arrays.asList(fields));

```

The field name is case-insensitive (Field and FIELD - the same names) and must be unique in the template. The related field must be associated with the early defined field. If these conditions don't meet, the exception is thrown.

## Template tables

Template table is set by [TemplateTable](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateTable) class with the following constructors:

```java
TemplateTable(TemplateTableLayout layout, String name, Integer pageIndex)
TemplateTable(TemplateTableParameters parameters, String name, Integer pageIndex)

```

Template table can be set by detector parameters or table layout. If the page index is omitted, tables are extracted from every document page. It's useful in the cases when the document contains pages with the same layout (pages differ only by data).

[TemplateTableParameters](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateTableParameters) class has the following constructors:

```java
TemplateTableParameters(Rectangle rectangle, Iterable<Double> verticalSeparators)
TemplateTableParameters(Rectangle rectangle, Iterable<Double> verticalSeparators, Boolean hasMergedCells, Integer minRowCount, Integer minColumnCount, Integer minVerticalSpace)

```

Each of parameters is optional. The most easy way to define a table is to set the rectangular area of the table and columns separators:

```java
TemplateTableParameters parameters = new TemplateTableParameters(
    new Rectangle(new Point(175, 350), new Size(400, 200)),
    java.util.Arrays.asList(new Double[] { 185.0, 370.0, 425.0, 485.0, 545.0 }));

```

If a template table is set by detector parameters, the table is detected automatically:

```java
TemplateTableParameters parameters = new TemplateTableParameters(
    new Rectangle(new Point(175, 350), new Size(400, 200)),
    java.util.Arrays.asList(new Double[] { 185.0, 370.0, 425.0, 485.0, 545.0 }));

TemplateTable table = new TemplateTable(parameters, "Details", 0);

// Create a document template
Template template = new Template(java.util.Arrays.asList(new TemplateItem[] { table }));

```

Template table is set by table layout if the table can't be detected automatically:

```java
TemplateTableLayout layout = new TemplateTableLayout(
    java.util.Arrays.asList(new Double[] { 50.0, 95.0, 275.0 }),
    java.util.Arrays.asList(new Double[] { 325.0, 340.0, 365.0 }));

TemplateTable table = new TemplateTable(layout, "Details", null);
Template template = new Template(java.util.Arrays.asList(new TemplateItem[] { table }));

```

These collections represent bounds of columns and rows. For example, for 2x2 table there are 3 vertical and 3 horizontal separators:

```
---------
|   |   |
---------
|   |   |
---------

```

[moveTo(Point)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateTableLayout#moveTo(com.groupdocs.parser.data.Point)) method is used to move Table Layout. 

For example, a document has tables on each page (or a set of documents with a table on the page). These tables differ by position and content, but have the same columns and rows. In this case a user can define [TemplateTableLayout](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateTableLayout) object at (0, 0) once and then move it to the location of the definite table.

If the table position depends on the other object of the page, a user can define [TemplateTableLayout](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateTableLayout) object based on template document and then move it according to an anchor object. For example, if this is a summary table and it is followed by details table (which can contain a different count of rows). In this case a user can define [TemplateTableLayout](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateTableLayout) object on template document (with the known details table rectangle) and then move [TemplateTableLayout](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateTableLayout) object according to the difference of details table rectangle of template and real document.

[moveTo(Point)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.templates/TemplateTableLayout#moveTo(com.groupdocs.parser.data.Point)) method returns a copy of the current object. A user can pass any coordinates (even negative - then layout will be moved to the left/top).

## Template barcodes

Template barcodes work in the same way as a template field with the fixed position. The following example shows how to define a template barcode field:

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

## Complex template example

This example shows the template which is used to parse the following invoice:

![](/parser/java/images/working-with-templates_6.jpg)

```java
// Create detector parameters for "Details" table
TemplateTableParameters detailsTableParameters = new TemplateTableParameters(new Rectangle(new Point(35, 320), new Size(530, 55)), null);
// Create detector parameters for "Summary" table
TemplateTableParameters summaryTableParameters = new TemplateTableParameters(new Rectangle(new Point(330, 385), new Size(220, 65)), null);
// Create a collection of template items
TemplateItem[] templateItems = new TemplateItem[]
        {
                new TemplateField(new TemplateFixedPosition(new Rectangle(new Point(35, 135), new Size(100, 10))), "FromCompany"),
                new TemplateField(new TemplateFixedPosition(new Rectangle(new Point(35, 150), new Size(100, 35))), "FromAddress"),
                new TemplateField(new TemplateFixedPosition(new Rectangle(new Point(35, 190), new Size(150, 2))), "FromEmail"),
                new TemplateField(new TemplateFixedPosition(new Rectangle(new Point(35, 250), new Size(100, 2))), "ToCompany"),
                new TemplateField(new TemplateFixedPosition(new Rectangle(new Point(35, 260), new Size(100, 15))), "ToAddress"),
                new TemplateField(new TemplateFixedPosition(new Rectangle(new Point(35, 290), new Size(150, 2))), "ToEmail"),
                new TemplateField(new TemplateRegexPosition("Invoice Number"), "InvoiceNumber"),
                new TemplateField(new TemplateLinkedPosition(
                        "InvoiceNumber",
                        new Size(200, 15),
                        new TemplateLinkedPositionEdges(false, false, true, false)),
                        "InvoiceNumberValue"),
                new TemplateField(new TemplateRegexPosition("Order Number"), "InvoiceOrder"),
                new TemplateField(new TemplateLinkedPosition(
                        "InvoiceOrder",
                        new Size(200, 15),
                        new TemplateLinkedPositionEdges(false, false, true, false)),
                        "InvoiceOrderValue"),
                new TemplateField(new TemplateRegexPosition("Invoice Date"), "InvoiceDate"),
                new TemplateField(new TemplateLinkedPosition(
                        "InvoiceDate",
                        new Size(200, 15),
                        new TemplateLinkedPositionEdges(false, false, true, false)),
                        "InvoiceDateValue"),
                new TemplateField(new TemplateRegexPosition("Due Date"), "DueDate"),
                new TemplateField(new TemplateLinkedPosition(
                        "DueDate",
                        new Size(200, 15),
                        new TemplateLinkedPositionEdges(false, false, true, false)),
                        "DueDateValue"),
                new TemplateField(new TemplateRegexPosition("Total Due"), "TotalDue"),
                new TemplateField(new TemplateLinkedPosition(
                        "TotalDue",
                        new Size(200, 15),
                        new TemplateLinkedPositionEdges(false, false, true, false)),
                        "TotalDueValue"),
                new TemplateTable(detailsTableParameters, "details", null),
                new TemplateTable(summaryTableParameters, "summary", null)
        };
// Create a document template
Template template = new Template(java.util.Arrays.asList(templateItems));


```

## Images and scanned PDFs

[ParseByTemplateOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ParseByTemplateOptions) class is used with [parseByTemplate(Template, ParseByTemplateOptions)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#parseByTemplate(com.groupdocs.parser.templates.Template,%20com.groupdocs.parser.options.ParseByTemplateOptions)) method. It has the following members:

| Member | Description |
| --- | --- |
| getPageIndex / setPageIndex | The zero-based index of the page to parse; *null* to parse all pages. |
| getUseOcr / setUseOcr | The value that indicates whether OCR is requested for template parsing. |
| getOcrOptions / setOcrOptions | The [OcrOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/OcrOptions) object for the OCR connector. |

{{< alert style="warning" >}}
GroupDocs.Parser for Java 26.10 doesn't support template-based parsing of images and scanned PDFs. Parsing by template isn't available for image files ([parseByTemplate](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/Parser#parseByTemplate(com.groupdocs.parser.templates.Template)) returns *null* for them), and the `useOcr` and `ocrOptions` values of [ParseByTemplateOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/ParseByTemplateOptions) are accepted but not used yet: the template is applied to the text layer of the document, and only the page index is taken into account. To get data from scanned documents, create the parser with an OCR connector and extract text areas with OCR (see [OCR Usage Basics]({{< ref "parser/java/developer-guide/advanced-usage/using-ocr/ocr-usage-basics.md" >}})).
{{< /alert >}}

The following example shows how to parse only the first page of the document by the template:

```java
// Load a document template from the file
Template template = Template.load("template.xml");

// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SamplePdfWithBarcodes)) {
    // Parse the first page of the document by the template
    DocumentData data = parser.parseByTemplate(template, new ParseByTemplateOptions(0, false));

    // Check if parsing by template is supported
    if (data == null) {
        System.out.println("Parsing by template isn't supported.");
        return;
    }

    // Print all extracted data
    for (int i = 0; i < data.getCount(); i++) {
        // Print the field's name and text
        System.out.println(data.get(i).getName() + ": " + data.get(i).getText());
    }
}
```

## Template serialization

Templates can be loaded from XML files instead of creating them in the code. Use [Template.load(String)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.templates/Template#load(java.lang.String)) to load a template from a file or [Template.load(InputStream)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.templates/Template#load(java.io.InputStream)) to load it from a stream. If the XML doesn't represent a template in a valid format, [GroupDocsParserException](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.exceptions/GroupDocsParserException) is thrown.

```java
// Load a document template from the file
Template template = Template.load("template.xml");

// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SamplePdfWithBarcodes)) {
    // Parse the document by the template
    DocumentData data = parser.parseByTemplate(template);

    // Print all extracted data
    for (int i = 0; i < data.getCount(); i++) {
        // Print the field's name and text
        System.out.println(data.get(i).getName() + ": " + data.get(i).getText());
    }
}
```

Templates can be saved from the code with [save(String)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.templates/Template#save(java.lang.String)) or [save(OutputStream)](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.templates/Template#save(java.io.OutputStream)) methods:

```java
// Create a collection of template items
TemplateItem[] templateItems = new TemplateItem[]
        {
                // ... items were omitted, see samples above
        };

// Create a document template
Template template = new Template(java.util.Arrays.asList(templateItems));
// Save the document template to the file
template.save("template.xml");
```

You can also create templates in any text editor. The following example shows a template with one fixed field:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE template SYSTEM "template.v1.dtd">
<template version="2.0">
  <items>
    <field name="Address">
      <fixed position="35;160" size="110;20"/>
    </field>
  </items>
</template>
```

Points and sizes are written as two numbers separated by a semicolon. The `save` method writes version `2.0`; the `load` method accepts versions `1.0` and `2.0`. Start hand-written files with the same `DOCTYPE` declaration that `save` writes: `template.v1.dtd` is resolved from a copy bundled in the library, nothing is downloaded. XML DTD file is the following:

```xml
<!ELEMENT template (items)>
<!ATTLIST template version CDATA #REQUIRED>
<!ATTLIST template rectangleTolerance CDATA #IMPLIED>

<!ELEMENT items (barcode|field|table)*>

<!ELEMENT barcode EMPTY>
<!ATTLIST barcode name CDATA #REQUIRED>
<!ATTLIST barcode pageIndex CDATA #IMPLIED>
<!ATTLIST barcode position CDATA #REQUIRED>
<!ATTLIST barcode size CDATA #REQUIRED>

<!ELEMENT field (fixed|regex|link)>
<!ATTLIST field name CDATA #REQUIRED>
<!ATTLIST field pageIndex CDATA #IMPLIED>

<!ELEMENT fixed EMPTY>
<!ATTLIST fixed position CDATA #REQUIRED>
<!ATTLIST fixed size CDATA #REQUIRED>

<!ELEMENT regex EMPTY>
<!ATTLIST regex expression CDATA #REQUIRED>
<!ATTLIST regex matchCase CDATA #IMPLIED>

<!ELEMENT link EMPTY>
<!ATTLIST link fieldName CDATA #REQUIRED>
<!ATTLIST link searchArea CDATA #REQUIRED>
<!ATTLIST link edges CDATA #REQUIRED>
<!ATTLIST link autoScale CDATA #IMPLIED>

<!ELEMENT table (layout|parameters)>
<!ATTLIST table name CDATA #REQUIRED>
<!ATTLIST table pageIndex CDATA #IMPLIED>

<!ELEMENT layout EMPTY>
<!ATTLIST layout verticalSeparators CDATA #REQUIRED>
<!ATTLIST layout horizontalSeparators CDATA #REQUIRED>

<!ELEMENT parameters EMPTY>
<!ATTLIST parameters position CDATA #REQUIRED>
<!ATTLIST parameters size CDATA #REQUIRED>
<!ATTLIST parameters verticalSeparators CDATA #IMPLIED>
<!ATTLIST parameters hasMergedCells CDATA #IMPLIED>
<!ATTLIST parameters minRowCount CDATA #IMPLIED>
<!ATTLIST parameters minColumnCount CDATA #IMPLIED>
<!ATTLIST parameters minVerticalSpace CDATA #IMPLIED>
```

In addition to the attributes declared in the DTD, `barcode`, `field` and `table` elements can have an optional `pageWidth` attribute. `save` writes it for template items created with a page width, and `load` reads it.

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).