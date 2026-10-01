---
id: extract-text-in-accurate-mode
url: parser/nodejs-java/extract-text-in-accurate-mode
title: Extract text in Accurate Mode
weight: 1
description: "Extract text from a whole document or a single page in Accurate mode with GroupDocs.Parser for Node.js via Java."
keywords: extract text, extract text in Accurate mode, extract text from page, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser provides the functionality to extract text from documents.

The **Accurate** mode is the default text extraction mode: the text quality is the best possible.

You can extract the whole document text or only the text of a document page.

To extract text in Accurate mode, the `getText` method of the `Parser` class is used:

```javascript
parser.getText();          // the whole document
parser.getText(pageIndex); // a single page (zero-based index)
```

The methods return an instance of the `TextReader` class with the extracted text, or `null` if text extraction isn't supported for the document. To retrieve the total number of document pages, use the `getDocumentInfo` method (see below).

`TextReader` has the following members:

| Member | Description |
| --- | --- |
| `readLine()` | Reads a line of characters from the text reader and returns the data as a string. |
| `readToEnd()` | Reads all characters from the current position to the end of the text reader and returns them as one string. |
| `close()` | Releases the resources used by the reader. |

## Extract text

Here are the steps to extract text from the document:

*   Instantiate the `Parser` object for the initial document;
*   Call the `getText` method and obtain the `TextReader` object;
*   Check if `reader` isn't `null` (text extraction is supported for the document);
*   Read the text from `reader`.

The following example shows how to extract text from a document:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.pdf');
try {
  // Extract a text into the reader
  const reader = parser.getText();
  // If text extraction isn't supported, a reader is null
  if (reader == null) {
    console.log("Text extraction isn't supported");
  } else {
    try {
      // Print a text from the document
      console.log(reader.readToEnd());
    } finally {
      reader.close();
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

## Extract text from page

Here are the steps to extract text from a document page:

*   Instantiate the `Parser` object for the initial document;
*   Call `parser.getFeatures().isText()` to check if text extraction is supported for the document;
*   Call the `getDocumentInfo` method to get the number of pages;
*   Call the `getText(pageIndex)` method with the page index and obtain the `TextReader` object;
*   Read the text from `reader`.

The following example shows how to extract text from document pages:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

function run() {
  // Create an instance of Parser class
  const parser = new groupdocs.Parser('sample.pdf');
  try {
    // Check if the document supports text extraction
    if (!parser.getFeatures().isText()) {
      console.log("Document doesn't support text extraction.");
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
    for (let p = 0; p < documentInfo.getPageCount(); p++) {
      // Print a page number
      console.log(`Page ${p + 1}/${documentInfo.getPageCount()}`);
      // Extract a text into the reader
      // We ignore null-checking as we have checked text extraction feature support earlier
      const reader = parser.getText(p);
      try {
        // Print a text from the document page
        console.log(reader.readToEnd());
      } finally {
        reader.close();
      }
    }
  } finally {
    parser.close();
  }
}

run();
process.exit(0);
```

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
