---
id: search-text
url: parser/nodejs-java/search-text
title: Search text
weight: 4
description: "Search text in documents by keyword or regular expression, with highlights and page numbers, using GroupDocs.Parser for Node.js via Java."
keywords: search text, search text in documents, regular expression search, SearchOptions, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser provides the functionality to search text in documents by the `search` method:

```javascript
parser.search(keyword);          // see the limitation below
parser.search(keyword, options); // options is a SearchOptions object
```

The `keyword` parameter can contain a text or a regular expression. The method returns a Java `Iterable` of `SearchResult` objects (or `null` if search isn't supported for the document); iterate it with `iterator()`, `hasNext()` and `next()`. Every `SearchResult` represents one occurrence of the keyword in the document text and has the following members:

| Member | Description |
| --- | --- |
| `getPosition()` | A zero-based index of the start position of the search result. When the search is performed by pages, this index starts from the document page start. |
| `getPageIndex()` | The page index where the text is found. |
| `getText()` | The found text. |
| `getLeftHighlightItem()` | The left highlight. |
| `getRightHighlightItem()` | The right highlight. |

{{< alert style="warning" >}}
In version 26.9 iterating the results of a search that is **not** performed by pages throws
`java.lang.NullPointerException: Cannot invoke "...Nullable.hasValue()" because "<parameter1>" is null`.
This affects `search(keyword)` and the `SearchOptions` constructors without the `searchByPages` parameter.
Always pass `SearchOptions` with `searchByPages` set to `true`, as in the examples below. In this case
`getPosition()` is counted from the start of the page given by `getPageIndex()`.
{{< /alert >}}

`SearchOptions` is used to customize a search. It has the following members:

| Member | Description |
| --- | --- |
| `isMatchCase()` | The value that indicates whether a text case isn't ignored. |
| `isMatchWholeWord()` | The value that indicates whether text search is limited by the whole word. |
| `isUseRegularExpression()` | The value that indicates whether a regular expression is used. |
| `isSearchByPages()` | The value that indicates whether the search is performed by pages. |
| `getLeftHighlightOptions()` | The options for the left highlight. |
| `getRightHighlightOptions()` | The options for the right highlight. |

The constructors that set `searchByPages` are:

```javascript
new groupdocs.SearchOptions(matchCase, matchWholeWord, useRegularExpression, searchByPages);
new groupdocs.SearchOptions(matchCase, matchWholeWord, useRegularExpression, searchByPages,
  leftHighlightOptions, rightHighlightOptions);
```

## Search text by keyword

Here are the steps to search a keyword in the document:

*   Instantiate the `Parser` object for the initial document;
*   Instantiate the `SearchOptions` object with `searchByPages` set to `true`;
*   Call the `search` method and obtain the collection of `SearchResult` objects;
*   Check if the collection isn't `null` (search is supported for the document);
*   Iterate through the collection and get the position and text.

The following example shows how to find a keyword in a document:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.pdf');
try {
  // Search a keyword (matchCase = false, matchWholeWord = false, useRegularExpression = false, searchByPages = true)
  const sr = parser.search('lorem', new groupdocs.SearchOptions(false, false, false, true));
  // Check if search is supported
  if (sr == null) {
    console.log("Search isn't supported");
  } else {
    // Iterate over search results
    const it = sr.iterator();
    while (it.hasNext()) {
      const s = it.next();
      // Print an index and found text
      console.log(`At ${s.getPosition()}: ${s.getText()}`);
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

## Search text by regular expression

Here are the steps to search with a regular expression in the document:

*   Instantiate the `Parser` object for the initial document;
*   Instantiate the `SearchOptions` object with `useRegularExpression` and `searchByPages` set to `true`;
*   Call the `search` method and obtain the collection of `SearchResult` objects;
*   Check if the collection isn't `null` (search is supported for the document);
*   Iterate through the collection and get the position and text.

The following example shows how to search with a regular expression in a document:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.pdf');
try {
  // Search with a regular expression with case matching
  const sr = parser.search('[0-9]+', new groupdocs.SearchOptions(true, false, true, true));
  // Check if search is supported
  if (sr == null) {
    console.log("Search isn't supported");
  } else {
    // Iterate over search results
    const it = sr.iterator();
    while (it.hasNext()) {
      const s = it.next();
      // Print an index and found text
      console.log(`At ${s.getPosition()}: ${s.getText()}`);
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

## Search text with highlights

Here are the steps to search a text with highlights:

*   Instantiate the `Parser` object for the initial document;
*   Instantiate the `HighlightOptions` object with the parameters for the highlight extraction (see [Extract highlights](/parser/nodejs-java/extract-highlights/));
*   Instantiate the `SearchOptions` object with the search parameters and the left and right highlight options;
*   Call the `search` method and obtain the collection of `SearchResult` objects;
*   Check if the collection isn't `null` (search is supported for the document);
*   Iterate through the collection and get the text and highlights.

The following example shows how to search a text with highlights:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.pdf');
try {
  // Highlights are limited to 15 characters
  const highlightOptions = new groupdocs.HighlightOptions(15);
  // Search a keyword with left and right highlights
  const sr = parser.search('lorem', new groupdocs.SearchOptions(true, false, false, true, highlightOptions, highlightOptions));
  // Check if search is supported
  if (sr == null) {
    console.log("Search isn't supported");
  } else {
    // Iterate over search results
    const it = sr.iterator();
    while (it.hasNext()) {
      const s = it.next();
      // Print the found text and highlights
      console.log(`${s.getLeftHighlightItem().getText()}${s.getText()}${s.getRightHighlightItem().getText()}`);
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

## Search text with page numbers

Here are the steps to search a text with page numbers:

*   Instantiate the `Parser` object for the initial document;
*   Instantiate the `SearchOptions` object with `searchByPages` set to `true`;
*   Call the `search` method and obtain the collection of `SearchResult` objects;
*   Check if the collection isn't `null` (search is supported for the document);
*   Iterate through the collection and get the position, text and page index.

The following example shows how to search a text with page numbers:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.pdf');
try {
  // Search a keyword with page numbers
  const sr = parser.search('lorem', new groupdocs.SearchOptions(false, false, false, true));
  // Check if search is supported
  if (sr == null) {
    console.log("Search isn't supported");
  } else {
    // Iterate over search results
    const it = sr.iterator();
    while (it.hasNext()) {
      const s = it.next();
      // Print an index, page number and found text
      console.log(`At ${s.getPosition()} (${s.getPageIndex()}): ${s.getText()}`);
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

The output for `sample.pdf` looks like this (the position is counted from the page start):

```
At 0 (0): Lorem
At 7 (0): Lorem
At 471 (0): lorem
At 893 (0): lorem
At 1274 (0): lorem
At 154 (1): lorem
```

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
