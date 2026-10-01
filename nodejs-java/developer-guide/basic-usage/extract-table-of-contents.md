---
id: extract-table-of-contents
url: parser/nodejs-java/extract-table-of-contents
title: Extract table of contents
weight: 11
description: "Extract the table of contents from Microsoft Word, PDF documents and ebooks (CHM, EPUB) in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: extract table of contents, TOC, DOC, DOCX, CHM, EPUB, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser allows to extract table of contents from Microsoft Word (DOC, DOCX etc), PDF documents and Ebooks.

## Extract table of contents

To extract table of contents from documents, use the `getToc()` method:

```javascript
parser.getToc(); // returns a Java Iterable of TocItem objects or null
```

The `TocItem` class has the following members:

| Member | Description |
| --- | --- |
| `getDepth()` | The depth level. |
| `getPageIndex()` | The page index, or `null` if the item doesn't refer to a page. |
| `getText()` | The text. |
| `extractText()` | Extracts a text from the document to which the `TocItem` object refers. For details, see [Extract text by table of contents item](/parser/nodejs-java/extract-text-by-table-of-contents-item/). |

Here are the steps to extract table of contents from the document:

*   Instantiate the `Parser` object for the initial document;
*   Call the `getToc()` method and obtain the collection of `TocItem` objects;
*   Check if *collection* isn't *null* (table of contents extraction is supported for the document);
*   Iterate through the collection and get the page index to extract a page text from the document.

The following example shows how to extract table of contents from an EPUB file:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.epub');
try {
  if (!parser.getFeatures().isText()) {
    // Check if text extraction is supported
    console.log("Text extraction isn't supported.");
  } else if (!parser.getFeatures().isToc()) {
    // Check if toc extraction is supported
    console.log("Toc extraction isn't supported.");
  } else {
    // Get table of contents
    const toc = parser.getToc();
    // Iterate over items
    const it = toc.iterator();
    while (it.hasNext()) {
      const item = it.next();
      // Print the Toc text
      console.log(item.getText());
      // Check if page index has a value
      const pageIndex = item.getPageIndex();
      if (pageIndex === null) {
        continue;
      }
      // Extract a page text
      const reader = parser.getText(pageIndex);
      try {
        console.log(reader.readToEnd());
      } finally {
        reader.close();
      }
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
