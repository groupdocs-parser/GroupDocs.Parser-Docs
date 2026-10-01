---
id: extract-tables-from-document-page
url: parser/nodejs-java/extract-tables-from-document-page
title: Extract tables from document page
weight: 4
description: "Learn how to extract tables from a specific document page in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: extract tables, extract tables from document page, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser provides the functionality to extract tables from a document page by the `getTables(int, PageTableAreaOptions)` method:

```javascript
parser.getTables(pageIndex, options) // returns Iterable<PageTableArea>
```

This method returns a Java `Iterable` collection of `PageTableArea` objects:

| Member | Description |
| --- | --- |
| `getRectangle()` | The rectangular area that bounds the table. |
| `getPage()` | The page information (page index and page size). |
| `getRowCount()` | The total number of the table rows. |
| `getColumnCount()` | The total number of the table columns. |
| `getCell(row, column)` | The table cell by row and column indexes. |
| `getRowHeight(row)` | The row height. |
| `getColumnWidth(column)` | The column width. |

`getTables(int, PageTableAreaOptions)` accepts a `PageTableAreaOptions` object that contains a `TemplateTableLayout` object with the table layout (see [Working with templates](/parser/nodejs-java/working-with-templates/) for more details).

Here are the steps to extract tables from document pages:

* Instantiate the `Parser` object for the initial document;
* Check if the document supports table extraction;
* Call the `getTables(int, PageTableAreaOptions)` method with the page index and obtain the collection of `PageTableArea` objects;
* Iterate through the collection and print table cells.

The following example shows how to extract tables from each document page:

```javascript
const java = require('java');
const groupdocs = require('@groupdocs/groupdocs.parser');

// Convert a JavaScript array of numbers to java.util.List<Double>
function toDoubleList(values) {
  const list = java.newInstanceSync('java.util.ArrayList');
  values.forEach((v) => list.add(java.newDouble(v)));
  return list;
}

// Create an instance of Parser class
const parser = new groupdocs.Parser('invoice_pages.pdf');
try {
  // Check if the document supports table extraction
  if (!parser.getFeatures().isTables()) {
    console.log("Document doesn't support tables extraction.");
  } else {
    // Create the layout of tables
    const layout = new groupdocs.TemplateTableLayout(
      toDoubleList([50.0, 95.0, 275.0, 415.0, 485.0, 545.0]),
      toDoubleList([325.0, 340.0, 365.0, 395.0]));
    // Create the options for table extraction
    const options = new groupdocs.PageTableAreaOptions(layout);
    // Get the document info
    const documentInfo = parser.getDocumentInfo();
    const pageCount = documentInfo.getPageCount();
    // Iterate over pages
    for (let pageIndex = 0; pageIndex < pageCount; pageIndex++) {
      // Print a page number
      console.log(`Page ${pageIndex + 1}/${pageCount}`);
      // Extract tables from the document page
      const it = parser.getTables(pageIndex, options).iterator();
      // Iterate over tables
      while (it.hasNext()) {
        const t = it.next();
        // Iterate over rows
        for (let row = 0; row < t.getRowCount(); row++) {
          let line = '';
          // Iterate over columns
          for (let column = 0; column < t.getColumnCount(); column++) {
            // Get the table cell
            const cell = t.getCell(row, column);
            if (cell != null) {
              // Add the table cell text
              line += cell.getText() + ' | ';
            }
          }
          console.log(line);
        }
        console.log();
      }
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

The output for `invoice_pages.pdf`:

```text
Page 1/2
Hrs/Qty | Service | Rate/Price | Adjust | Sub Total | 
1.00 | Web Design
This is a sample description... | $85.00 | 0.00% | $85.00 | 

Page 2/2
Hrs/Qty | Service | Rate/Price | Adjust | Sub Total | 
1.00 | SEO
This is a sample description... | $152.00 | 0.00% | $152.00 | 
1.00 | SMM
This is a sample description... | $41.00 | 0.00% | $41.00 | 
```

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
