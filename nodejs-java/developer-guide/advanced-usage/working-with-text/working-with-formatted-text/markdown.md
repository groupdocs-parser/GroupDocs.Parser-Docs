---
id: markdown
url: parser/nodejs-java/markdown
title: Markdown
weight: 4
description: "Extract document text as Markdown with GroupDocs.Parser for Node.js via Java."
keywords: extract Markdown formatted text, extract Markdown formatted text from document, FormattedTextMode.Markdown, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
The following example shows how to extract Markdown formatted text:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.docx');
try {
  // Extract a formatted text into the reader
  const reader = parser.getFormattedText(new groupdocs.FormattedTextOptions(groupdocs.FormattedTextMode.Markdown));
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

The API supports the following formatting:

*   Bold text
*   Italic text
*   Hyperlinks
*   Headings
*   Numbering and bullets lists
*   Tables

The following Microsoft Word document is used as input document:

![](/parser/nodejs-java/images/markdown.png)

The following Markdown document is extracted using the example above:

![](/parser/nodejs-java/images/markdown_1.png)

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
