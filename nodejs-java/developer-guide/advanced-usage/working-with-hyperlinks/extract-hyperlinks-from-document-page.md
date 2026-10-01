---
id: extract-hyperlinks-from-document-page
url: parser/nodejs-java/extract-hyperlinks-from-document-page
title: Extract hyperlinks from document page
weight: 4
description: "Learn how to extract hyperlinks from a specific document page in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: extract hyperlinks from document page, extract hyperlinks, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser provides the functionality to extract hyperlinks from a document page by the `getHyperlinks(int)` method:

```javascript
parser.getHyperlinks(pageIndex) // returns Iterable<PageHyperlinkArea>
```

This method returns a Java `Iterable` collection of `PageHyperlinkArea` objects:

| Member | Description |
| --- | --- |
| `getPage()` | The page that contains the hyperlink. |
| `getRectangle()` | The rectangular area on the page that contains the hyperlink. |
| `getText()` | The hyperlink text. |
| `getUrl()` | The hyperlink URL. |

Here are the steps to extract hyperlinks from the document page:

* Instantiate the `Parser` object for the initial document;
* Check if the document supports hyperlink extraction;
* Call the `getHyperlinks(int)` method with the page index and obtain the collection of `PageHyperlinkArea` objects;
* Iterate through the collection and get a hyperlink text and URL.

The following example shows how to extract hyperlinks from each document page:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('Hyperlinks.pdf');
try {
  // Check if the document supports hyperlink extraction
  if (!parser.getFeatures().isHyperlinks()) {
    console.log("Document doesn't support hyperlink extraction.");
  } else {
    // Get the document info
    const documentInfo = parser.getDocumentInfo();
    const pageCount = documentInfo.getPageCount();
    // Iterate over pages
    for (let pageIndex = 0; pageIndex < pageCount; pageIndex++) {
      // Print a page number
      console.log(`Page ${pageIndex + 1}/${pageCount}`);
      // Extract hyperlinks from the document page
      const it = parser.getHyperlinks(pageIndex).iterator();
      // Iterate over hyperlinks
      while (it.hasNext()) {
        const h = it.next();
        // Print the hyperlink text
        console.log(h.getText());
        // Print the hyperlink URL
        console.log(h.getUrl());
        console.log();
      }
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

The output for `Hyperlinks.pdf`:

```text
Page 1/2
Lorem ipsum
http://link1/

pharetra nonummy
http://link11/

Page 2/2
Donec ut est
http://link2/

Maecenas odio
http://link21/
```

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
