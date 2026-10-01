---
id: extract-formatted-text-from-document
url: parser/nodejs-java/extract-formatted-text-from-document
title: Extract formatted text from document
weight: 1
description: "Extract the whole document text as HTML, Markdown or plain text with GroupDocs.Parser for Node.js via Java."
keywords: extract formatted text, extract formatted text from documents, FormattedTextOptions, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser provides the functionality to extract formatted text from documents by the `getFormattedText` method:

```javascript
parser.getFormattedText(options); // options is a FormattedTextOptions object
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

Here are the steps to extract HTML formatted text from the document:

*   Instantiate the `Parser` object for the initial document;
*   Instantiate `FormattedTextOptions` with the HTML text mode;
*   Call the `getFormattedText` method and obtain the `TextReader` object;
*   Check if `reader` isn't `null` (formatted text extraction is supported for the document);
*   Read the text from `reader`.

The following example shows how to extract a document text as HTML:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.docx');
try {
  // Extract a formatted text into the reader
  const reader = parser.getFormattedText(new groupdocs.FormattedTextOptions(groupdocs.FormattedTextMode.Html));
  // If formatted text extraction isn't supported, a reader is null
  if (reader == null) {
    console.log("Formatted text extraction isn't supported");
  } else {
    try {
      // Print a formatted text from the document
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

To extract the text of a single page, see [Extract formatted text from document page](/parser/nodejs-java/extract-formatted-text-from-document-page/).

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
