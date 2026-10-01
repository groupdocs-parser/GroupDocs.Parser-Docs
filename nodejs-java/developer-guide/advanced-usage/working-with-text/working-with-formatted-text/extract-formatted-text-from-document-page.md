---
id: extract-formatted-text-from-document-page
url: parser/nodejs-java/extract-formatted-text-from-document-page
title: Extract formatted text from document page
weight: 2
description: "Extract the text of a document page as HTML, Markdown or plain text with GroupDocs.Parser for Node.js via Java."
keywords: extract formatted text, extract formatted text from document page, FormattedTextOptions, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser provides the functionality to extract formatted text from a document page by the `getFormattedText` method:

```javascript
parser.getFormattedText(pageIndex, options); // options is a FormattedTextOptions object
```

The method returns an instance of the `TextReader` class with the extracted text, or `null` if formatted text extraction isn't supported for the document. `FormattedTextOptions` has the following constructor:

```javascript
new groupdocs.FormattedTextOptions(mode); // mode is a FormattedTextMode value
```

`FormattedTextMode` has the following members:

| Member | Description |
| --- | --- |
| `groupdocs.FormattedTextMode.Html` | [HTML](/parser/nodejs-java/html/) format. |
| `groupdocs.FormattedTextMode.Markdown` | [Markdown](/parser/nodejs-java/markdown/) format. |
| `groupdocs.FormattedTextMode.PlainText` | [Plain text](/parser/nodejs-java/plain-text/) format. |

`TextReader` has the following members:

| Member | Description |
| --- | --- |
| `readLine()` | Reads a line of characters from the text reader and returns the data as a string. |
| `readToEnd()` | Reads all characters from the current position to the end of the text reader and returns them as one string. |
| `close()` | Releases the resources used by the reader. |

Here are the steps to extract Markdown formatted page text from the document:

*   Instantiate the `Parser` object for the initial document;
*   Call `parser.getFeatures().isFormattedText()` to check if formatted text extraction is supported for the document;
*   Call the `getDocumentInfo` method to get the number of pages;
*   Instantiate `FormattedTextOptions` with the Markdown text mode;
*   Call the `getFormattedText(pageIndex, options)` method with the page index and obtain the `TextReader` object;
*   Read the text from `reader`.

The following example shows how to extract document pages as Markdown text:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

function run() {
  // Create an instance of Parser class
  const parser = new groupdocs.Parser('sample.docx');
  try {
    // Check if the document supports formatted text extraction
    if (!parser.getFeatures().isFormattedText()) {
      console.log("Document doesn't support formatted text extraction.");
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
      // Extract a formatted text into the reader
      // We ignore null-checking as we have checked formatted text extraction feature support earlier
      const reader = parser.getFormattedText(p, new groupdocs.FormattedTextOptions(groupdocs.FormattedTextMode.Markdown));
      try {
        // Print a formatted text from the document page
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
