---
id: parse-data-from-documents
url: parser/nodejs-java/parse-data-from-documents
title: Parse data from documents
weight: 4
description: "Parse data from PDF, Microsoft Word, Excel and LibreOffice documents by a user-defined template in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: Document parsing, parse by template, template, DOC, DOCX, XLS, XLSX, PDF, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser provides the Document Parser feature that allows you to extract data from documents of various formats including PDF, Microsoft Word, Excel, LibreOffice formats etc. (see the [full supported list](/parser/nodejs-java/supported-document-formats/)).

With the Document Parsing feature you can easily solve business automation tasks with the data extracted from your documents.

Using this feature is straightforward. Simply define a template programmatically and apply it.

## Parse data from documents

GroupDocs.Parser provides the functionality to extract data from documents by the `parseByTemplate(template)` method:

```javascript
parser.parseByTemplate(template); // returns DocumentData or null
```

This method parses data from the document by a user-generated template.

Here are the steps to parse data from the document by a user-generated template:

*   Instantiate the `Parser` object for the initial document;
*   Instantiate the `Template` object with the user-generated template;
*   Call the `parseByTemplate(template)` method and obtain the `DocumentData` object;
*   Check if *data* isn't *null* (parse by template is supported for the document);
*   Iterate over field data to obtain the extracted data.

The `Template` constructor accepts a Java collection of template items, so the example creates a `java.util.ArrayList` and adds the items to it. Use `java.instanceOf(object, className)` from the `java` package to check the Java type of a field value.

The following example shows how to parse data from the document by a user-generated template:

```javascript
const java = require('java');
const groupdocs = require('@groupdocs/groupdocs.parser');

function rect(x, y, width, height) {
  return new groupdocs.Rectangle(new groupdocs.Point(x, y), new groupdocs.Size(width, height));
}

function fixedField(x, y, width, height, name) {
  return new groupdocs.TemplateField(new groupdocs.TemplateFixedPosition(rect(x, y, width, height)), name);
}

function linkedField(linkedFieldName, name) {
  // The value is located to the right of the linked field
  const edges = new groupdocs.TemplateLinkedPositionEdges(false, false, true, false);
  return new groupdocs.TemplateField(
    new groupdocs.TemplateLinkedPosition(linkedFieldName, new groupdocs.Size(200, 15), edges), name);
}

function getTemplate() {
  // Create detector parameters for "Details" table
  const detailsTableParameters = new groupdocs.TemplateTableParameters(rect(35, 320, 530, 55), null);
  // Create detector parameters for "Summary" table
  const summaryTableParameters = new groupdocs.TemplateTableParameters(rect(330, 385, 220, 65), null);
  // Create a collection of template items
  const templateItems = [
    fixedField(35, 135, 100, 10, 'FromCompany'),
    fixedField(35, 150, 100, 35, 'FromAddress'),
    fixedField(35, 190, 150, 2, 'FromEmail'),
    fixedField(35, 250, 100, 2, 'ToCompany'),
    fixedField(35, 260, 100, 15, 'ToAddress'),
    fixedField(35, 290, 150, 2, 'ToEmail'),
    new groupdocs.TemplateField(new groupdocs.TemplateRegexPosition('Invoice Number'), 'InvoiceNumber'),
    linkedField('InvoiceNumber', 'InvoiceNumberValue'),
    new groupdocs.TemplateField(new groupdocs.TemplateRegexPosition('Order Number'), 'InvoiceOrder'),
    linkedField('InvoiceOrder', 'InvoiceOrderValue'),
    new groupdocs.TemplateField(new groupdocs.TemplateRegexPosition('Invoice Date'), 'InvoiceDate'),
    linkedField('InvoiceDate', 'InvoiceDateValue'),
    new groupdocs.TemplateField(new groupdocs.TemplateRegexPosition('Due Date'), 'DueDate'),
    linkedField('DueDate', 'DueDateValue'),
    new groupdocs.TemplateField(new groupdocs.TemplateRegexPosition('Total Due'), 'TotalDue'),
    linkedField('TotalDue', 'TotalDueValue'),
    new groupdocs.TemplateTable(detailsTableParameters, 'details', null),
    new groupdocs.TemplateTable(summaryTableParameters, 'summary', null),
  ];
  // Create a document template
  const items = new (java.import('java.util.ArrayList'))();
  templateItems.forEach((item) => items.add(item));
  return new groupdocs.Template(items);
}

// Create an instance of Parser class
const parser = new groupdocs.Parser('invoice.pdf');
try {
  // Parse the document by the template
  const data = parser.parseByTemplate(getTemplate());
  // Check if parsing by template is supported
  if (data === null) {
    console.log("Parse Document by Template isn't supported.");
  } else {
    // Print extracted fields
    for (let i = 0; i < data.getCount(); i++) {
      const field = data.get(i);
      const area = field.getPageArea();
      // Check if the field value is a text area
      const isTextArea = area !== null && java.instanceOf(area, 'com.groupdocs.parser.data.PageTextArea');
      console.log(`${field.getName()}: ${isTextArea ? area.getText() : 'Not a template field'}`);
    }
  }
} finally {
  parser.close();
}

process.exit(0);
```

The beginning of the output looks like this:

```text
FROMCOMPANY: DEMO - Sliced Invoices
FROMADDRESS: Suite 5A-1204
123 Somewhere Street
Your City AZ 12345
FROMEMAIL: admin@slicedinvoices.com
...
INVOICENUMBERVALUE: INV-3337
...
TOTALDUEVALUE: $93.50
DETAILS: Not a template field
SUMMARY: Not a template field
```

The `details` and `summary` fields contain tables (`PageTableArea` objects) rather than text areas.

## More resources

### Advanced usage topics

To learn more about template building and working with extracted data please refer to the following guides:

*   [Working with templates](/parser/nodejs-java/working-with-templates/)
*   [Working with data extracted by template](/parser/nodejs-java/working-with-data-extracted-by-template/)

### Free online document parser App

Along with the full-featured library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
