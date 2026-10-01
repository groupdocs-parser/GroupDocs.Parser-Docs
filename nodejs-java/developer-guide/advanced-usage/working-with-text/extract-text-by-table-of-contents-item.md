---
id: extract-text-by-table-of-contents-item
url: parser/nodejs-java/extract-text-by-table-of-contents-item
title: Extract text by table of contents item
weight: 9
description: "Extract the text of a document chapter by its table of contents item with GroupDocs.Parser for Node.js via Java."
keywords: extract text, extract text by table of contents item, TocItem, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser provides the functionality to extract text by an item of the table of contents. This feature is supported for Word Processing, PDF, ePUB and CHM documents (for more details, see [Supported Document Formats](/parser/nodejs-java/supported-document-formats/)).

Text is extracted by the `extractText` method of the `TocItem` class:

```javascript
// Print the text of the chapter
const reader = tocItem.extractText();
try {
  console.log('----');
  console.log(reader.readToEnd());
} finally {
  reader.close();
}
```

This method returns the text of the chapter to which the table of contents item refers (without sub-chapters). For example, "Heading 1.2" from the page

![](/parser/nodejs-java/images/extract-text-by-table-of-contents-item.png)

returns the following text:

![](/parser/nodejs-java/images/extract-text-by-table-of-contents-item_1.png)

"Heading 2" from the page:

![](/parser/nodejs-java/images/extract-text-by-table-of-contents-item_2.png)

returns the following text:

![](/parser/nodejs-java/images/extract-text-by-table-of-contents-item_3.png)

{{< alert style="warning" >}}
`java.lang.UnsupportedOperationException` is thrown if `tocItem.getPageIndex()` is `null`.
{{< /alert >}}

Here are the steps to extract text by an item of the table of contents:

*   Instantiate the `Parser` object for the initial document;
*   Call the `getToc` method and obtain the collection of `TocItem` objects;
*   Check if the collection isn't `null` (table of contents extraction is supported for the document);
*   Iterate through the collection and extract the text.

The following example shows how to extract text by items of the table of contents:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('SampleWithToc.docx');
try {
  // Get table of contents
  const tocItems = parser.getToc();
  // Check if toc extraction is supported
  if (tocItems == null) {
    console.log("Table of contents extraction isn't supported");
  } else {
    // Iterate over items
    const it = tocItems.iterator();
    while (it.hasNext()) {
      const tocItem = it.next();
      // Print the text of the chapter
      const reader = tocItem.extractText();
      try {
        console.log('----');
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

### Free online document parser App

Along with the full-featured library we provide simple but powerful free apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
