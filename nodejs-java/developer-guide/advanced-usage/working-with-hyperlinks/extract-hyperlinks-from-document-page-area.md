---
id: extract-hyperlinks-from-document-page-area
url: parser/nodejs-java/extract-hyperlinks-from-document-page-area
title: Extract hyperlinks from document page area
weight: 4
description: "Learn how to extract hyperlinks from a rectangular page area in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: extract hyperlinks, extract hyperlinks from document page area, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser provides the functionality to extract hyperlinks from a document page area by the `getHyperlinks(PageAreaOptions)` and `getHyperlinks(int, PageAreaOptions)` methods:

```javascript
parser.getHyperlinks(options)            // returns Iterable<PageHyperlinkArea>
parser.getHyperlinks(pageIndex, options) // returns Iterable<PageHyperlinkArea>
```

These methods return a Java `Iterable` collection of `PageHyperlinkArea` objects:

| Member | Description |
| --- | --- |
| `getPage()` | The page that contains the hyperlink. |
| `getRectangle()` | The rectangular area on the page that contains the hyperlink. |
| `getText()` | The hyperlink text. |
| `getUrl()` | The hyperlink URL. |

Here are the steps to extract hyperlinks from the document page area:

* Instantiate the `Parser` object for the initial document;
* Check if the document supports hyperlink extraction;
* Instantiate `PageAreaOptions` with the rectangular area;
* Call the `getHyperlinks(PageAreaOptions)` method and obtain the collection of `PageHyperlinkArea` objects;
* Iterate through the collection and get a hyperlink text and URL.

The following example shows how to extract hyperlinks from the document page area:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('Hyperlinks.pdf');
try {
  // Check if the document supports hyperlink extraction
  if (!parser.getFeatures().isHyperlinks()) {
    console.log("Document doesn't support hyperlink extraction.");
  } else {
    // Create the options which are used for hyperlink extraction
    const options = new groupdocs.PageAreaOptions(
      new groupdocs.Rectangle(new groupdocs.Point(380, 90), new groupdocs.Size(150, 50)));
    // Extract hyperlinks from the document page area
    const it = parser.getHyperlinks(options).iterator();
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
} finally {
  parser.close();
}
process.exit(0);
```

The output for `Hyperlinks.pdf`:

```text
pharetra nonummy
http://link11/

Maecenas odio
http://link21/
```

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
