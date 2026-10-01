---
id: extract-highlights
url: parser/nodejs-java/extract-highlights
title: Extract highlights
weight: 3
description: "Extract a highlight (a text fragment around a position) from documents with GroupDocs.Parser for Node.js via Java."
keywords: extract highlights, extract highlights from documents, HighlightOptions, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser provides the functionality to extract a highlight (a part of the text which is usually used to explain the context of the found text in the [search functionality](/parser/nodejs-java/search-text/)) from documents by the `getHighlight` method:

```javascript
parser.getHighlight(position, isDirect, options); // returns HighlightItem or null
```

The `position` parameter defines the start position from which the highlight is extracted. The `isDirect` parameter indicates whether highlight extraction is direct: `true` if the highlight is extracted to the right of the position; otherwise, `false`. The `HighlightOptions` parameter is used to define the end of the highlight.

The `HighlightOptions` class has the following constructors:

```javascript
// Highlight is limited to maxLength text length.
new groupdocs.HighlightOptions(maxLength);
// Highlight is limited to the start (or the end) of a text line (or maxLength text length - if set; pass null otherwise).
new groupdocs.HighlightOptions(maxLength, isLineLimited);
// Highlight is limited to word count (or maxLength text length - if set; pass null otherwise).
new groupdocs.HighlightOptions(maxLength, wordCount);
// General constructor
new groupdocs.HighlightOptions(maxLength, wordCount, isLineLimited);
```

The `HighlightItem` class has the following members:

| Member | Description |
| --- | --- |
| `getPosition()` | The position in the document text. |
| `getText()` | The highlight text. |

Here are the steps to extract a highlight from the document:

*   Instantiate the `Parser` object for the initial document;
*   Instantiate the `HighlightOptions` object with the extraction parameters;
*   Call the `getHighlight` method and obtain the `HighlightItem` object;
*   Check if `HighlightItem` isn't `null` (highlight extraction is supported for the document);
*   Call `getPosition()` and `getText()`.

The following example shows how to extract a highlight limited to 3 characters:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.pdf');
try {
  // Extract a highlight which starts at position 2 and contains up to 3 characters
  const hl = parser.getHighlight(2, true, new groupdocs.HighlightOptions(3));
  // Check if highlight extraction is supported
  if (hl == null) {
    console.log("Highlight extraction isn't supported");
  } else {
    // Print an extracted highlight
    console.log(`At ${hl.getPosition()}: ${hl.getText()}`);
  }
} finally {
  parser.close();
}
process.exit(0);
```

To limit the highlight by the number of words instead of characters, pass `null` as the length: `new groupdocs.HighlightOptions(null, 3)`.

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
