---
id: search-text-in-html-documents
url: parser/java/search-text-in-html-documents
title: Search text in HTML documents
weight: 2
description: "Learn how to search a keyword in HTML documents search(String) method is used. This method returns the collection of SearchResult objects."
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---
To search a keyword in HTML documents [search(String)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#search(java.lang.String)) method is used. This method returns the collection of [SearchResult](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/SearchResult) objects. For details, see [Search Text]({{< ref "parser/java/developer-guide/advanced-usage/working-with-text/search-text.md" >}}).

{{< alert style="warning" >}}
Parsing does not execute JavaScript; for dynamically generated HTML, pre-render or extract the actual text content before searching.
{{< /alert >}}

Here are the steps to search a keyword in HTML document:

*   Instantiate [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object for the initial document;
*   Call [search(String)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#search(java.lang.String)) method and obtain the collection of [SearchResult](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/SearchResult) objects;
*   Iterate through the collection and get the position and text.

{{< alert style="warning" >}}
[search(String)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#search(java.lang.String)) method returns *null* value if search isn't supported for the document. For example, text extraction isn't supported for Zip archive. Therefore, for Zip archive [search(String)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#search(java.lang.String)) method returns *null*. For empty HTML document [search(String)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#search(java.lang.String)) method returns an empty collection.
{{< /alert >}}

The following example shows how to find a keyword in HTML document:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleHtml)) {
    // Search a keyword:
    Iterable<SearchResult> sr = parser.search("Sub1");
    // Iterate over search results
    for (SearchResult s : sr) {
        // Print an index and found text:
        System.out.println(String.format("At %d: %s", s.getPosition(), s.getText()));
    }
}
```

[search(String, SearchOptions)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#search(java.lang.String,%20com.groupdocs.parser.options.SearchOptions)) method is used for the advanced search in HTML documents - like search with regular expressions.  [SearchOptions](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/SearchOptions) parameter is used to customize a search.

Here are the steps to search with a regular expression in HTML document:

*   Instantiate [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object for the initial document;
*   Instantiate [SearchOptions](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/SearchOptions) object with the parameters for the search;
*   Call [search(String, SearchOptions)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#search(java.lang.String,%20com.groupdocs.parser.options.SearchOptions)) method and obtain the collection of [SearchResult](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/SearchResult) objects;
*   Iterate through the collection and get the position and text.

The following example shows how to search with a regular expression in HTML document:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleHtml)) {
    // Search with a regular expression with case matching
    Iterable<SearchResult> sr = parser.search("Sub[0-9]", new SearchOptions(true, false, true));
    // Iterate over search results
    for (SearchResult s : sr) {
        // Print an index and found text:
        System.out.println(String.format("At %d: %s", s.getPosition(), s.getText()));
    }
}
```

## Null vs. Empty Collection Results

The [search](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#search(java.lang.String)) method behaves differently depending on the document type and its text content:

- **Returns `null`** → when the document type does not support searching (e.g., ZIP archives or other formats for which [isSearch](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isSearch()) returns `false`).
- **Returns an empty collection** → when the document supports searching (like HTML) but the keyword isn't found, for example because the document contains no searchable text.

The method returns `Iterable<SearchResult>`, which has no size property: use `iterator().hasNext()` to check whether the collection is empty.

### Example 1: Search in a non-searchable document (returns `null`)

```java
try (Parser parser = new Parser(Constants.SampleZip)) { // ZIP archives are not searchable
    Iterable<SearchResult> results = parser.search("example");
    if (results == null) {
        System.out.println("Search returned null: document type does not support searching.");
    }
}
```

### Example 2: Search in an empty HTML file (returns empty collection)

```java
try (Parser parser = new Parser("empty.html")) { // Empty HTML without text
    Iterable<SearchResult> results = parser.search("example");
    if (results != null && !results.iterator().hasNext()) {
        System.out.println("Search returned empty collection: no text found in the document.");
    }
}
```

### Example 3: Search in a valid HTML file (returns results)

```java
try (Parser parser = new Parser(Constants.SampleHtml)) { // Contains text
    Iterable<SearchResult> results = parser.search("Sub1");
    if (results != null) {
        for (SearchResult result : results) {
            System.out.println(String.format("Found: '%s' at position %d", result.getText(), result.getPosition()));
        }
    }
}
```

## Preprocessing Recommendations

GroupDocs.Parser works with the **text content** of HTML documents.
It does **not** execute JavaScript. If your HTML file relies on JavaScript to generate content dynamically, the parser may return an empty collection.

To improve results, consider the following preprocessing steps:

### 1. Use Pre-rendered HTML
Ensure that the HTML file you provide already contains the visible text content.
If your HTML is generated by a web application, save or export the **fully rendered** version of the page before passing it to the parser.

### 2. Remove Unnecessary Markup
If possible, simplify your HTML by removing scripts, styles, or metadata that do not affect the visible text.
This reduces noise and makes search results more accurate.

### 3. Normalize Encodings
Verify that the HTML file uses a standard encoding (such as UTF-8).
Unexpected or inconsistent encodings may cause missing or unreadable text when searching.

### 4. Validate Text Presence
If search results are empty, open the HTML file in a text editor to confirm whether actual text is present in the markup.
If the content only appears after running JavaScript in a browser, it will not be searchable.

**Summary**:
Provide **static, text-based HTML** whenever possible.
Dynamic or script-driven HTML will not yield results unless converted into plain text or pre-rendered before parsing.

If the HTML document refers to external resources (images, style sheets), see [Handle loading of external resources]({{< ref "parser/java/developer-guide/advanced-usage/loading/handle-loading-of-external-resources.md" >}}) and [Network access and data privacy]({{< ref "parser/java/getting-started/network-access-and-data-privacy.md" >}}).

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).