---
id: working-with-data-extracted-by-template
url: parser/nodejs-java/working-with-data-extracted-by-template
title: Working with data extracted by template
weight: 102
description: "Learn how to read fields and tables from DocumentData returned by parseByTemplate in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: extracted data, DocumentData, FieldData, parse by template, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

## DocumentData class

Data extracted by the `parseByTemplate` method are stored in the instance of the `DocumentData` class:

| Member | Description |
| --- | --- |
| `getCount()` | The total number of the data fields. |
| `get(int)` | The data field. |
| `getFieldsByName(String)` | Returns the Java list of data fields where the name is equal to fieldName. |
| `iterator()` | Returns the Java iterator over the data fields. |

The `FieldData` class has the following members:

| Member | Description |
| --- | --- |
| `getName()` | The field name. |
| `getPageIndex()` | The page index. |
| `getPageArea()` | The value of the field. |
| `getLinkedField()` | The linked field. |

Field data are stored in the `getPageArea()` property. Depending on the type of the value it can contain an instance of the `PageTextArea`, `PageTableArea` or `PageBarcodeArea` classes. JavaScript `instanceof` doesn't work with Java objects; check the type with `java.instanceOf`:

```javascript
const java = require('java');

// Get the field data
const field = data.get(i);
// Check if the field data contains a text
if (java.instanceOf(field.getPageArea(), 'com.groupdocs.parser.data.PageTextArea')) {
  // Print the field value
  console.log(field.getPageArea().getText());
}
```

The `PageTextArea` class represents a text block on the page. This class has the following members:

| Member | Description |
| --- | --- |
| `getRectangle()` | The rectangular area that bounds the text area. |
| `getPage()` | The page information (page index and page size). |
| `getText()` | The value of the text area. |
| `getBaseLine()` | The base line of the text area. |
| `getTextStyle()` | The style of the text block (like font name, font size etc.) |
| `getAreas()` | The collection of child text areas. |

The text area can be single or composite. In the first case it contains a text which is bounded by a rectangular area. In the second case it contains other text areas; text and table properties are calculated by child text areas.

The `PageTableArea` class represents a table. This class has the following members:

| Member | Description |
| --- | --- |
| `getRectangle()` | The rectangular area that bounds the table. |
| `getPage()` | The page information (page index and page size). |
| `getRowCount()` | The total number of the table rows. |
| `getColumnCount()` | The total number of the table columns. |
| `getCell(int, int)` | The table cell by row and column indexes. |
| `getRowHeight(int)` | Returns the row height. |
| `getColumnWidth(int)` | Returns the column width. |

There are two ways to work with fields data.

## Iterate through fields

The following example shows how to iterate via extracted field data:

```javascript
const java = require('java');
const groupdocs = require('@groupdocs/groupdocs.parser');
const ArrayList = java.import('java.util.ArrayList');

// Define a "price" field
const priceField = new groupdocs.TemplateField(
  new groupdocs.TemplateRegexPosition('\\$\\d+(.\\d+)?'),
  'Price');
// Define a "email" field
const emailField = new groupdocs.TemplateField(
  new groupdocs.TemplateRegexPosition('[a-z]+\\@[a-z]+.[a-z]+'),
  'Email');
// Create a template
const items = new ArrayList();
items.add(priceField);
items.add(emailField);
const template = new groupdocs.Template(items);

// Create an instance of Parser class
const parser = new groupdocs.Parser('invoice.pdf');
try {
  // Parse the document by the template
  const data = parser.parseByTemplate(template);
  // Print all extracted data
  for (let i = 0; i < data.getCount(); i++) {
    const field = data.get(i);
    // As we have defined only text fields in the template,
    // the page area is expected to be PageTextArea
    const area = field.getPageArea();
    const value = java.instanceOf(area, 'com.groupdocs.parser.data.PageTextArea')
      ? area.getText()
      : 'Not a template field';
    // Print field name and value
    console.log(field.getName() + ': ' + value);
  }
} finally {
  parser.close();
}
process.exit(0);
```

The output for `invoice.pdf`:

```
PRICE: $93.50
PRICE: $85.00
PRICE: $85.00
PRICE: $85.00
PRICE: $8.50
PRICE: $93.50
EMAIL: admin@slicedinvoices.com
EMAIL: admin@slicedinvoices.com
EMAIL: test@test.com
```

## Get field by name

The following example shows how to get fields by the name:

```javascript
const java = require('java');
const groupdocs = require('@groupdocs/groupdocs.parser');
const ArrayList = java.import('java.util.ArrayList');

// Define "price" and "email" fields
const items = new ArrayList();
items.add(new groupdocs.TemplateField(new groupdocs.TemplateRegexPosition('\\$\\d+(.\\d+)?'), 'Price'));
items.add(new groupdocs.TemplateField(new groupdocs.TemplateRegexPosition('[a-z]+\\@[a-z]+.[a-z]+'), 'Email'));
// Create a template
const template = new groupdocs.Template(items);

// Print values of the fields with the name
function printFields(data, name) {
  const it = data.getFieldsByName(name).iterator();
  while (it.hasNext()) {
    const area = it.next().getPageArea();
    console.log(java.instanceOf(area, 'com.groupdocs.parser.data.PageTextArea')
      ? area.getText()
      : 'Not a template field');
  }
}

// Create an instance of Parser class
const parser = new groupdocs.Parser('invoice.pdf');
try {
  // Parse the document by the template
  const data = parser.parseByTemplate(template);
  // Print prices
  console.log('Prices:');
  printFields(data, 'Price');
  // Print emails
  console.log('Emails:');
  printFields(data, 'Email');
} finally {
  parser.close();
}
process.exit(0);
```

This functionality allows to iterate all data fields and select the most suitable of them. For example, if more than one text value meets the condition of the regular expression, a user can iterate over them and select the most suitable one.

## Working with tables

The following example shows how to work with extracted tables:

```javascript
const java = require('java');
const groupdocs = require('@groupdocs/groupdocs.parser');
const ArrayList = java.import('java.util.ArrayList');

// Create a table template with the parameters
const table = new groupdocs.TemplateTable(
  new groupdocs.TemplateTableParameters(
    new groupdocs.Rectangle(new groupdocs.Point(35, 320), new groupdocs.Size(530, 55)), null),
  'Details',
  null);
// Create a template
const items = new ArrayList();
items.add(table);
const template = new groupdocs.Template(items);

// Create an instance of Parser class
const parser = new groupdocs.Parser('invoice.pdf');
try {
  // Parse the document by the template
  const data = parser.parseByTemplate(template);
  // Print all extracted data
  for (let i = 0; i < data.getCount(); i++) {
    console.log(data.get(i).getName() + ':');
    // Check if the field is a table
    const area = data.get(i).getPageArea();
    if (!java.instanceOf(area, 'com.groupdocs.parser.data.PageTableArea')) {
      continue;
    }
    // Iterate via table rows
    for (let row = 0; row < area.getRowCount(); row++) {
      const cells = [];
      // Iterate via table columns
      for (let column = 0; column < area.getColumnCount(); column++) {
        // Get the cell value
        const cellValue = area.getCell(row, column).getPageArea();
        cells.push(java.instanceOf(cellValue, 'com.groupdocs.parser.data.PageTextArea') ? cellValue.getText() : '');
      }
      // Print the row; cells are separated by tabs
      console.log(cells.join('\t'));
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
