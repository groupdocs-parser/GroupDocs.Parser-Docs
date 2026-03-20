---  
id: working-with-templates  
url: parser/net/working-with-templates  
title: Working with templates  
weight: 101  
version: 23.5  
description: "Complete guide to creating and using templates for structured data extraction with GroupDocs.Parser for .NET. Learn template‑based extraction for invoices, forms, and documents in C#."  
keywords: template-based extraction, document templates, template parsing, invoice parsing, structured data extraction, template fields, template tables  
productName: GroupDocs.Parser for .NET  
hideChildren: False  
toc: true  
tags: csharp, parser, templates, template-extraction, structured-data, v23.5  
---  

Document template is set by the **[Template](https://reference.groupdocs.com/net/parser/groupdocs.parser.templates/template)** class. It contains template items – fields and tables. Each item has a unique (in the template bounds) name and an optional page index – a value that represents the index of the page where the template item is located; *null* if the template item can appear on any page.  

## Template fields  

The template field is set by the **[TemplateField](https://reference.groupdocs.com/net/parser/groupdocs.parser.templates/templatefield)** class. In the current API the constructor signature is:

```csharp
// name – a unique template item name
// pageArea – a PageArea object that contains the rectangle (and optionally the page) where the field is located
new TemplateField(string name, PageArea pageArea);
```

| Parameter | Description |
| --- | --- |
| **name** | A unique template item name. |
| **pageArea** | A **PageArea** object that defines the rectangle (and optionally the page) where the field is located. |

**TemplatePosition** is an abstract base class. The following classes are used to set template positions:

*   **[TemplateFixedPosition](https://reference.groupdocs.com/net/parser/groupdocs.parser.templates/templatefixedposition)** – provides a fixed rectangular area.  
*   **[TemplateRegexPosition](https://reference.groupdocs.com/net/parser/groupdocs.parser.templates/templateregexposition)** – uses a regular expression.  
*   **[TemplateLinkedPosition](https://reference.groupdocs.com/net/parser/groupdocs.parser.templates/templatelinkedposition)** – uses a linked field.  

### TemplateFixedPosition  

This is the simplest way to define the field position. It requires a rectangular area on the page that bounds the field value. All the text that is contained (even partially) in the rectangular area will be extracted as a value:

```csharp
// Create a fixed template field with the name "Address" that is bounded by a rectangle
// at the position (35, 160) and with the size (110, 20)
TemplateField templateField = new TemplateField(
    "Address",
    new PageArea
    {
        Rectangle = new Rectangle(new Point(35, 160), new Size(110, 20))
    });
```

It is recommended to define a rectangular area above (below) the centre of the line that is below (above) the selected area, in order to avoid excessive extraction of the text. For example:

| Template definition | Result |
| --- | --- |
| ![](/parser/net/images/working-with-templates.png) | Extracts only one line `67890` |
| ![](/parser/net/images/working-with-templates_1.png) | Extracts two lines `4321 First Street`<br>`Anytown, State ZIP` |
| ![](/parser/net/images/working-with-templates_2.png) | Extracts four lines `Company Name`<br>`4321 First Street`<br>`Anytown, State ZIP`<br>`Date: 06/02/2019` |

### TemplateRegexPosition  

This way to define the field position allows you to find a field value by a regular expression. For example, if the document contains **“Invoice Number INV‑12345”** then the template field can be defined as follows:

```csharp
// Create a regex template field with the name "InvoiceNumber"
TemplateField templateField = new TemplateField(
    "InvoiceNumber",
    new PageArea
    {
        // The rectangle is not required for a regex position, so it can be left null
        Rectangle = null
    })
{
    // Attach the regex position to the field
    Position = new TemplateRegexPosition("Invoice Number\\s+[A-Z0-9\\-]+")
};
```

In this case the entire string is extracted. To extract only a part of the string the regular‑expression group **“value”** is used:

```csharp
TemplateField templateField = new TemplateField(
    "InvoiceNumber",
    new PageArea { Rectangle = null })
{
    Position = new TemplateRegexPosition("Invoice Number\\s+(?<value>[A-Z0-9\\-]+)")
};
```

Now the extracted value will be **“INV‑3337”**.

Regular‑expression fields can also be used as linked fields.

### TemplateLinkedPosition  

This way to define the field position allows you to find a field value by extracting a rectangular area around a linked field. For example, if it is known that the field with an invoice number is placed on the right of the **“Invoice Number”** string, the following code is used:

```csharp
// Create a regex template field to find the label "Invoice Number"
TemplateField invoiceLabel = new TemplateField(
    "Invoice",
    new PageArea { Rectangle = null })
{
    Position = new TemplateRegexPosition("Invoice Number")
};

// Create a related template field that extracts the value on the right of the label
TemplateField invoiceNumber = new TemplateField(
    "InvoiceNumber",
    new PageArea
    {
        // The rectangle will be calculated based on the linked field
        Rectangle = null
    })
{
    Position = new TemplateLinkedPosition(
        linkedFieldName: "Invoice",
        searchArea: new Size(100, 15),
        edges: new TemplateLinkedPositionEdges(false, false, true, false))
};
```

| Template definition | Result |
| --- | --- |
| ![](/parser/net/images/working-with-templates_3.png) | Extracts the text on the right of **“Invoice Number”**: `INV‑3337` |

To simplify the setting of the size of a linked field, the **[AutoScale](https://reference.groupdocs.com/net/parser/groupdocs.parser.templates/templatelinkedposition/properties/autoscale)** property can be used. When **AutoScale** is set to *true*, the size of the template field is scaled according to the related field:

```csharp
TemplateField invoiceNumber = new TemplateField(
    "InvoiceNumber",
    new PageArea { Rectangle = null })
{
    Position = new TemplateLinkedPosition(
        linkedFieldName: "Invoice",
        searchArea: new Size(100, 15),
        edges: new TemplateLinkedPositionEdges(false, false, true, false),
        autoScale: true)
};
```

| Template definition | Result |
| --- | --- |
| ![](/parser/net/images/working-with-templates_4.png) | Extracts the text on the right of **“Invoice Number”**: `INV‑3337` |

The side of the value extraction is set by the **[Edges](https://reference.groupdocs.com/net/parser/groupdocs.parser.templates/templatelinkedposition/properties/edges)** property, and the size of the rectangular area is set by the **[SearchArea](https://reference.groupdocs.com/net/parser/groupdocs.parser.templates/templatelinkedposition/properties/searcharea)** property. The position of the rectangular area depends on the side of the value extraction:

```text
Left:   (LinkedField.Rectangle.Left - SearchArea.Width,  LinkedField.Rectangle.Top)
Top:    (LinkedField.Rectangle.Left,                LinkedField.Rectangle.Top - SearchArea.Height)
Right:  (LinkedField.Rectangle.Right,               LinkedField.Rectangle.Top)
Bottom: (LinkedField.Rectangle.Left,                LinkedField.Rectangle.Bottom)
```

The related field can be any field that was previously defined in the template:

```csharp
// Create a regex template field that finds "From"
TemplateField fromField = new TemplateField(
    "From",
    new PageArea { Rectangle = null })
{
    Position = new TemplateRegexPosition("From")
};

// Create a related template field linked to "From" and placed under it
TemplateField fromCompany = new TemplateField(
    "FromCompany",
    new PageArea { Rectangle = null })
{
    Position = new TemplateLinkedPosition(
        linkedFieldName: "From",
        searchArea: new Size(100, 10),
        edges: new TemplateLinkedPositionEdges(false, false, false, true))
};

// Create a related template field linked to "FromCompany" and placed under it
TemplateField fromAddress = new TemplateField(
    "FromAddress",
    new PageArea { Rectangle = null })
{
    Position = new TemplateLinkedPosition(
        linkedFieldName: "FromCompany",
        searchArea: new Size(100, 30),
        edges: new TemplateLinkedPositionEdges(false, false, false, true))
};
```

| Template definition | Result |
| --- | --- |
| ![](/parser/net/images/working-with-templates_5.png) | Extraction order:<br>1️⃣ **From** (green)<br>2️⃣ **FromCompany** (yellow)<br>3️⃣ **FromAddress** (red) |

A linked field is empty if the related field does not have a value. If the related field has a value, the linked field will contain that value and a reference to the related field.

### Document template with fields  

An instance of the **[Template](https://reference.groupdocs.com/net/parser/groupdocs.parser.templates/template)** class is created by the constructor:

```csharp
// Accepts a collection of TemplateItem objects (fields, tables, barcodes, etc.)
Template(IEnumerable<TemplateItem> items)
```

```csharp
// Create an array of template fields
TemplateItem[] fields = new TemplateItem[]
{
    new TemplateField(
        "From",
        new PageArea { Rectangle = null })
    {
        Position = new TemplateRegexPosition("From")
    },
    new TemplateField(
        "FromCompany",
        new PageArea { Rectangle = null })
    {
        Position = new TemplateLinkedPosition(
            linkedFieldName: "From",
            searchArea: new Size(100, 10),
            edges: new TemplateLinkedPositionEdges(false, false, false, true))
    },
    new TemplateField(
        "FromAddress",
        new PageArea { Rectangle = null })
    {
        Position = new TemplateLinkedPosition(
            linkedFieldName: "FromCompany",
            searchArea: new Size(100, 30),
            edges: new TemplateLinkedPositionEdges(false, false, false, true))
    }
};

// Create a document template
Template template = new Template(fields);
```

The field name is case‑insensitive (e.g., **Field** and **FIELD** are treated as the same) and must be unique within the template. The related field must be defined before it is referenced; otherwise, an exception is thrown.

## Template tables  

Template tables are set by the **[TemplateTable](https://reference.groupdocs.com/net/parser/groupdocs.parser.templates/templatetable)** class with the following constructors:

```csharp
TemplateTable(TemplateTableLayout layout, string name, int? pageIndex = null)
TemplateTable(TemplateTableParameters parameters, string name, int? pageIndex = null)
```

A table can be defined either by detector parameters (**TemplateTableParameters**) or by an explicit layout (**TemplateTableLayout**). If the page index is omitted, tables are extracted from every page – useful when all pages share the same layout.

### Using detector parameters  

**TemplateTableParameters** has the following constructors:

```csharp
TemplateTableParameters(Rectangle rectangle, IEnumerable<double> verticalSeparators)
TemplateTableParameters(
    Rectangle rectangle,
    IEnumerable<double> verticalSeparators,
    bool? hasMergedCells,
    int? minRowCount,
    int? minColumnCount,
    int? minVerticalSpace)
```

The simplest way is to set the rectangular area of the table and the column separators:

```csharp
TemplateTableParameters parameters = new TemplateTableParameters(
    new Rectangle(new Point(175, 350), new Size(400, 200)),
    new double[] { 185, 370, 425, 485, 545 });
```

If a template table is defined by detector parameters, the table is detected automatically:

```csharp
TemplateTable table = new TemplateTable(parameters, "Details", 0);

// Create a document template
Template template = new Template(new TemplateItem[] { table });
```

### Using a table layout  

If the table cannot be detected automatically, you can define it by layout:

```csharp
TemplateTableLayout layout = new TemplateTableLayout(
    new double[] { 50, 95, 275 },          // vertical separators
    new double[] { 325, 340, 365 });       // horizontal separators

TemplateTable table = new TemplateTable(layout, "Details", null);
Template template = new Template(new TemplateItem[] { table });
```

The layout stores the bounds of columns and rows. For a **2 × 2** table there are three vertical and three horizontal separators:

```
---------
|   |   |
---------
|   |   |
---------
```

The **[MoveTo](https://reference.groupdocs.com/net/parser/groupdocs.parser.templates/templatetablelayout/methods/moveto)** method can be used to move a layout to a different position on the page.

### Example – moving a layout  

If a document contains tables on each page that share the same column structure but differ in position, you can define a layout once at (0, 0) and then move it to the required location:

```csharp
TemplateTableLayout baseLayout = new TemplateTableLayout(
    new double[] { 50, 95, 275 },
    new double[] { 325, 340, 365 });

// Move the layout to the position of the actual table on the page
TemplateTableLayout movedLayout = baseLayout.MoveTo(new Point(100, 200));

TemplateTable table = new TemplateTable(movedLayout, "Details", null);
Template template = new Template(new TemplateItem[] { table });
```

## Template barcodes  

Template barcodes work the same way as a fixed‑position field. The following example shows how to define a barcode field:

```csharp
// Define a barcode field
TemplateBarcode barcode = new TemplateBarcode(
    new Rectangle(new Point(590, 80), new Size(150, 150)),
    "QR");

// Create a template
Template template = new Template(new TemplateItem[] { barcode });

// Parse the document
using (Parser parser = new Parser(Constants.SamplePdfWithBarcodes))
{
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

## Complex template example  

The following example demonstrates a template that parses an invoice:

```csharp
// Detector parameters for the "Details" table
TemplateTableParameters detailsTableParameters = new TemplateTableParameters(
    new Rectangle(new Point(35, 320), new Size(530, 55)),
    null);

// Detector parameters for the "Summary" table
TemplateTableParameters summaryTableParameters = new TemplateTableParameters(
    new Rectangle(new Point(330, 385), new Size(220, 65)),
    null);

// Collection of template items
TemplateItem[] templateItems = new TemplateItem[]
{
    new TemplateField(
        "FromCompany",
        new PageArea { Rectangle = new Rectangle(new Point(35, 135), new Size(100, 10)) }),
    new TemplateField(
        "FromAddress",
        new PageArea { Rectangle = new Rectangle(new Point(35, 150), new Size(100, 35)) }),
    new TemplateField(
        "FromEmail",
        new PageArea { Rectangle = new Rectangle(new Point(35, 190), new Size(150, 2)) }),
    new TemplateField(
        "ToCompany",
        new PageArea { Rectangle = new Rectangle(new Point(35, 250), new Size(100, 2)) }),
    new TemplateField(
        "ToAddress",
        new PageArea { Rectangle = new Rectangle(new Point(35, 260), new Size(100, 15)) }),
    new TemplateField(
        "ToEmail",
        new PageArea { Rectangle = new Rectangle(new Point(35, 290), new Size(150, 2)) }),

    // Regex fields
    new TemplateField(
        "InvoiceNumber",
        new PageArea { Rectangle = null })
    {
        Position = new TemplateRegexPosition("Invoice Number")
    },
    new TemplateField(
        "InvoiceNumberValue",
        new PageArea { Rectangle = null })
    {
        Position = new TemplateLinkedPosition(
            linkedFieldName: "InvoiceNumber",
            searchArea: new Size(200, 15),
            edges: new TemplateLinkedPositionEdges(false, false, true, false))
    },

    new TemplateField(
        "InvoiceOrder",
        new PageArea { Rectangle = null })
    {
        Position = new TemplateRegexPosition("Order Number")
    },
    new TemplateField(
        "InvoiceOrderValue",
        new PageArea { Rectangle = null })
    {
        Position = new TemplateLinkedPosition(
            linkedFieldName: "InvoiceOrder",
            searchArea: new Size(200, 15),
            edges: new TemplateLinkedPositionEdges(false, false, true, false))
    },

    new TemplateField(
        "InvoiceDate",
        new PageArea { Rectangle = null })
    {
        Position = new TemplateRegexPosition("Invoice Date")
    },
    new TemplateField(
        "InvoiceDateValue",
        new PageArea { Rectangle = null })
    {
        Position = new TemplateLinkedPosition(
            linkedFieldName: "InvoiceDate",
            searchArea: new Size(200, 15),
            edges: new TemplateLinkedPositionEdges(false, false, true, false))
    },

    new TemplateField(
        "DueDate",
        new PageArea { Rectangle = null })
    {
        Position = new TemplateRegexPosition("Due Date")
    },
    new TemplateField(
        "DueDateValue",
        new PageArea { Rectangle = null })
    {
        Position = new TemplateLinkedPosition(
            linkedFieldName: "DueDate",
            searchArea: new Size(200, 15),
            edges: new TemplateLinkedPositionEdges(false, false, true, false))
    },

    new TemplateField(
        "TotalDue",
        new PageArea { Rectangle = null })
    {
        Position = new TemplateRegexPosition("Total Due")
    },
    new TemplateField(
        "TotalDueValue",
        new PageArea { Rectangle = null })
    {
        Position = new TemplateLinkedPosition(
            linkedFieldName: "TotalDue",
            searchArea: new Size(200, 15),
            edges: new TemplateLinkedPositionEdges(false, false, true, false))
    },

    // Tables
    new TemplateTable(detailsTableParameters, "details", null),
    new TemplateTable(summaryTableParameters, "summary", null)
};

// Create a document template
Template template = new Template(templateItems);
```

## Images and scanned PDFs  

Starting from version **25.2**, GroupDocs.Parser for .NET supports template‑based parsing for images and scanned PDF documents. By default it uses the built‑in OCR, which can be **replaced** with any other OCR engine. OCR is automatically enabled for images, but for scanned PDFs you need to use the overload that accepts **ParseByTemplateOptions**.

```csharp
// Load a document template from an XML file
Template template = Template.Load("template.xml");

// Create a parser instance
using (Parser parser = new Parser(Constants.SamplePdfWithBarcodes))
{
    // Parse the document by the template and force OCR usage
    var options = new ParseByTemplateOptions { UseOcr = true };
    DocumentData data = parser.ParseByTemplate(template, options);

    // Print all extracted data
    for (int i = 0; i < data.Count; i++)
    {
        Console.WriteLine($"{data[i].Name}: {data[i].Text}");
    }
}
```

> **Note:** This functionality has some limitations – only simple fields, tables, and barcodes are supported. Other field types will be ignored.

## Template serialization  

Since GroupDocs.Parser for .NET **23.12**, templates can be loaded from XML files instead of being created programmatically.

```csharp
// Load a document template from the file
Template template = Template.Load("template.xml");

// Parse the document
using (Parser parser = new Parser(Constants.SamplePdfWithBarcodes))
{
    DocumentData data = parser.ParseByTemplate(template);

    // Print all extracted data
    for (int i = 0; i < data.Count; i++)
    {
        Console.WriteLine($"{data[i].Name}: {data[i].Text}");
    }
}
```

Templates can also be saved from code:

```csharp
// Create a collection of template items (see the examples above)
TemplateItem[] templateItems = new TemplateItem[]
{
    // ... items were omitted for brevity
};

// Create a document template
Template template = new Template(templateItems);

// Save the template to an XML file
template.Save("template.xml");
```

You can also create templates manually in any text editor. The XML DTD file is as follows:

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

## More resources  

### GitHub examples  

You can run the code above and see the feature in action in our GitHub repositories:

*   **[GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)**  
*   **[GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)**  

### Free online document parser App  

Along with the full‑featured .NET library we provide a simple, yet powerful, free online application. You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails, and more with our free online **[Free Online Document Parser App](https://products.groupdocs.app/parser)**.