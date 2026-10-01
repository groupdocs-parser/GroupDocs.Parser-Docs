---
id: extract-formatted-text-from-documents
url: parser/nodejs-java/extract-formatted-text-from-documents
title: Extract formatted text from documents
weight: 6
description: "Extract text as HTML, Markdown or formatted plain text from emails, ebooks, Microsoft Office and LibreOffice documents in Node.js with GroupDocs.Parser."
keywords: Extract formatted text, HTML, Markdown, EPUB, FB2, CHM, DOC, DOCX, PPT, PPTX, XLS, XLSX, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser allows to extract formatted text from documents for those cases when simple plain text is not enough and you need to keep formatting like text style, table layout etc.

This feature allows to extract text with integrated HTML tags or Markdown syntax. Even PlainText mode allows to convert your documents to high quality text with integrated ASCII formatting symbols for tables, lists etc.

## Extract formatted text from documents

To extract a formatted text from documents simply call the `getFormattedText(options)` method:

```javascript
parser.getFormattedText(options); // returns TextReader or null
```

This method returns an instance of the `TextReader` class with an extracted text, or `null` if formatted text extraction isn't supported for the document. `FormattedTextOptions` has the following constructor:

```javascript
new groupdocs.FormattedTextOptions(mode);
```

The `FormattedTextMode` enumeration has the following members:

| Member | Description |
| --- | --- |
| `FormattedTextMode.Html` | HTML format. |
| `FormattedTextMode.Markdown` | Markdown format. |
| `FormattedTextMode.PlainText` | Plain text format. |

`TextReader` has the following members:

| Member | Description |
| --- | --- |
| `readLine()` | Reads a line of characters from the text reader and returns the data as a string. |
| `readToEnd()` | Reads all characters from the current position to the end of the text reader and returns them as one string. |
| `close()` | Releases resources used by the reader. |

Here are the steps to extract a HTML formatted text from the document:

*   Instantiate the `Parser` object for the initial document;
*   Instantiate `FormattedTextOptions` with HTML text mode;
*   Call the `getFormattedText(options)` method and obtain the `TextReader` object;
*   Check if *reader* isn't *null* (formatted text extraction is supported for the document);
*   Read a text from *reader*.

The following example shows how to extract a document text as HTML text:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.docx');
try {
  // Extract a formatted text into the reader
  const options = new groupdocs.FormattedTextOptions(groupdocs.FormattedTextMode.Html);
  const reader = parser.getFormattedText(options);
  // If formatted text extraction isn't supported, a reader is null
  if (reader === null) {
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

## More resources

### Advanced usage topics

To learn more about document data extraction features and get familiar how to extract text, images, forms and more, please refer to the [advanced usage section](/parser/nodejs-java/advanced-usage/).

### Free online document parser App

Along with the full-featured library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
