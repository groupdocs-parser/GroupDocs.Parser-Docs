---
id: extract-text-in-raw-mode
url: parser/nodejs-java/extract-text-in-raw-mode
title: Extract text in Raw Mode
weight: 2
description: "Extract text in the fast Raw mode from a whole document or a single page with GroupDocs.Parser for Node.js via Java."
keywords: extract text, extract text in Raw mode, raw text, TextOptions, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser provides the functionality to extract text from documents.

The **Raw** mode is the fastest text extraction mode: the performance is the best possible.

Raw mode usually retrieves text in worse quality than [Accurate mode](/parser/nodejs-java/extract-text-in-accurate-mode/), but in some cases performance is more important than quality.

You can extract the whole document text or only the text of a document page.

To extract text in Raw mode, pass a `TextOptions` object created with `true` to the `getText` method:

```javascript
parser.getText(new groupdocs.TextOptions(true));            // the whole document
parser.getText(pageIndex, new groupdocs.TextOptions(true)); // a single page (zero-based index)
```

The methods return an instance of the `TextReader` class with the extracted text, or `null` if text extraction isn't supported for the document. To retrieve the total number of document pages, use the `getDocumentInfo` method (see below).

{{< alert style="warning" >}}In Raw mode use `getRawPageCount()` of the document info instead of `getPageCount()` to avoid extra calculations.{{< /alert >}}

`TextReader` has the following members:

| Member | Description |
| --- | --- |
| `readLine()` | Reads a line of characters from the text reader and returns the data as a string. |
| `readToEnd()` | Reads all characters from the current position to the end of the text reader and returns them as one string. |
| `close()` | Releases the resources used by the reader. |

## Extract text

Here are the steps to extract raw text from the document:

*   Instantiate the `Parser` object for the initial document;
*   Instantiate the `TextOptions` object with the `true` parameter;
*   Call the `getText` method with `TextOptions` and obtain the `TextReader` object;
*   Check if `reader` isn't `null` (text extraction is supported for the document);
*   Read the text from `reader`.

The following example shows how to extract raw text from a document:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.pdf');
try {
  // Extract a raw text into the reader
  const reader = parser.getText(new groupdocs.TextOptions(true));
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

Here are the steps to extract raw text from a document page:

*   Instantiate the `Parser` object for the initial document;
*   Call `parser.getFeatures().isText()` to check if text extraction is supported for the document;
*   Call the `getDocumentInfo` method;
*   Use `getRawPageCount()` instead of `getPageCount()` to avoid extra calculations;
*   Call the `getText(pageIndex, options)` method with the page index and a `TextOptions` object created with `true`, and obtain the `TextReader` object;
*   Read the text from `reader`.

The following example shows how to extract raw text from document pages:

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
    if (documentInfo == null || documentInfo.getRawPageCount() === 0) {
      console.log('Document has no pages.');
      return;
    }
    // Iterate over pages
    for (let p = 0; p < documentInfo.getRawPageCount(); p++) {
      // Print a page number
      console.log(`Page ${p + 1}/${documentInfo.getRawPageCount()}`);
      // Extract a raw text into the reader
      // We ignore null-checking as we have checked text extraction feature support earlier
      const reader = parser.getText(p, new groupdocs.TextOptions(true));
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
