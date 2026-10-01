---
id: extract-text-areas
url: parser/nodejs-java/extract-text-areas
title: Extract text areas
weight: 7
description: "Extract text areas with their page rectangles from documents, pages or page regions with GroupDocs.Parser for Node.js via Java."
keywords: extract text areas, extract text areas from documents, PageTextAreaOptions, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser provides the functionality to extract text areas from documents by the `getTextAreas` method:

```javascript
parser.getTextAreas();                     // the whole document
parser.getTextAreas(options);              // the whole document, PageTextAreaOptions
parser.getTextAreas(pageIndex);            // a single page
parser.getTextAreas(pageIndex, options);   // a single page, PageTextAreaOptions
```

The method returns a Java `Iterable` of `PageTextArea` objects (or `null` if text areas extraction isn't supported for the document). `PageTextArea` has the following members:

| Member | Description |
| --- | --- |
| `getPage()` | The page that contains the text area. |
| `getRectangle()` | The rectangular area on the page that contains the text area. |
| `getText()` | The value of the text area. |
| `getBaseLine()` | The base line of the text area. |
| `getTextStyle()` | The text style of the text area. |
| `getAreas()` | The collection of child text areas. |

A text area represents a rectangular page area with a text. A text area can be simple or composite. A simple text area contains only a text and its `getAreas()` collection is always empty (not `null`). A composite text area doesn't have its own text: its text is calculated from the texts of its children returned by `getAreas()`.

## Extract text areas

Here are the steps to extract text areas from the whole document:

*   Instantiate the `Parser` object for the initial document;
*   Call the `getTextAreas` method and obtain the collection of `PageTextArea` objects;
*   Check if the collection isn't `null` (text areas extraction is supported for the document);
*   Iterate through the collection and get rectangles and text.

The following example shows how to extract all text areas from the whole document:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('images.pdf');
try {
  // Extract text areas
  const areas = parser.getTextAreas();
  // Check if text areas extraction is supported
  if (areas == null) {
    console.log("Page text areas extraction isn't supported");
  } else {
    // Iterate over page text areas
    const it = areas.iterator();
    while (it.hasNext()) {
      const a = it.next();
      // Print a page index, rectangle and text area value
      console.log(`Page: ${a.getPage().getIndex()}, R: ${a.getRectangle().toString()}, Text: ${a.getText()}`);
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

## Extract text areas from a document page

Here are the steps to extract text areas from a document page:

*   Instantiate the `Parser` object for the initial document;
*   Call `parser.getFeatures().isTextAreas()` to check if text areas extraction is supported for the document;
*   Call the `getTextAreas(pageIndex)` method with the page index and obtain the collection of `PageTextArea` objects;
*   Iterate through the collection and get rectangles and text.

The following example shows how to extract text areas from document pages:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

function run() {
  // Create an instance of Parser class
  const parser = new groupdocs.Parser('images.pdf');
  try {
    // Check if the document supports text areas extraction
    if (!parser.getFeatures().isTextAreas()) {
      console.log("Document doesn't support text areas extraction.");
      return;
    }
    // Get the document info
    const documentInfo = parser.getDocumentInfo();
    // Check if the document has pages
    if (documentInfo.getPageCount() === 0) {
      console.log('Document has no pages.');
      return;
    }
    // Iterate over pages
    for (let pageIndex = 0; pageIndex < documentInfo.getPageCount(); pageIndex++) {
      // Print a page number
      console.log(`Page ${pageIndex + 1}/${documentInfo.getPageCount()}`);
      // Iterate over page text areas
      // We ignore null-checking as we have checked text areas extraction feature support earlier
      const it = parser.getTextAreas(pageIndex).iterator();
      while (it.hasNext()) {
        const a = it.next();
        // Print a rectangle and text area value
        console.log(`R: ${a.getRectangle().toString()}, Text: ${a.getText()}`);
      }
    }
  } finally {
    parser.close();
  }
}

run();
process.exit(0);
```

## Extract text areas with options

`PageTextAreaOptions` is used to customize the text areas extraction process. It has the following members:

| Member | Description |
| --- | --- |
| `getRectangle()` | The rectangular area that contains a text area. |
| `getExpression()` | The regular expression. |
| `isMatchCase()` | The value that indicates whether a text case isn't ignored. |
| `isUniteSegments()` | The value that indicates whether segments are united. |
| `isIgnoreFormatting()` | The value that indicates whether text formatting is ignored. |

Here are the steps to extract text areas from the upper-left corner of pages:

*   Instantiate the `Parser` object for the initial document;
*   Instantiate `PageTextAreaOptions` with a regular expression and the rectangular area;
*   Call the `getTextAreas(options)` method and obtain the collection of `PageTextArea` objects;
*   Check if the collection isn't `null` (text areas extraction is supported for the document);
*   Iterate through the collection and get rectangles and text.

The following example shows how to extract only text areas that match a regular expression (a two-letter word surrounded by whitespace) from the upper-left corner of pages:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('images.pdf');
try {
  // Create the options which are used for text area extraction
  const options = new groupdocs.PageTextAreaOptions('\\s[a-z]{2}\\s',
    new groupdocs.Rectangle(new groupdocs.Point(0, 0), new groupdocs.Size(300, 100)));
  // Extract text areas which match the regular expression from the upper-left corner of a page
  const areas = parser.getTextAreas(options);
  // Check if text areas extraction is supported
  if (areas == null) {
    console.log("Page text areas extraction isn't supported");
  } else {
    // Iterate over page text areas
    const it = areas.iterator();
    while (it.hasNext()) {
      const a = it.next();
      // Print a page index, rectangle and text area value
      console.log(`Page: ${a.getPage().getIndex()}, R: ${a.getRectangle().toString()}, Text: ${a.getText()}`);
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
