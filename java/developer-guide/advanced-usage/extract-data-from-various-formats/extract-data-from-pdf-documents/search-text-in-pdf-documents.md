---
id: search-text-in-pdf-documents
url: parser/java/search-text-in-pdf-documents
title: Search text in PDF documents
weight: 6
description: ""
keywords: 
productName: GroupDocs.Parser for Java
hideChildren: False
---
To search a keyword in PDF documents [search(String)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#search(java.lang.String)) method is used. This method returns the collection of [SearchResult](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/SearchResult "class in com.groupdocs.parser.data") objects. For details, see [Search Text]({{< ref "parser/java/developer-guide/advanced-usage/working-with-text/search-text.md" >}}).

Here are the steps to search a keyword in PDF document:

*   Instantiate [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object for the initial document;
*   Call [search(String)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#search(java.lang.String)) method and obtain the collection of [SearchResult](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/SearchResult "class in com.groupdocs.parser.data") objects;
*   Iterate through the collection and get the position and text.

{{< alert style="warning" >}}
[search(String)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#search(java.lang.String)) method returns null value if search isn't supported for the document. For example, text extraction isn't supported for Zip archive. Therefore, for Zip archive [search(String)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#search(java.lang.String)) method returns null. For empty PDF document [search(String)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#search(java.lang.String)) method returns an empty collection.
{{< /alert >}}

The following example shows how to find a keyword in PDF document:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SamplePdf)) {
    // Search a keyword:
    Iterable<SearchResult> sr = parser.search("nunc");
    // Iterate over search results
    for (SearchResult s : sr) {
        // Print an index and found text:
        System.out.println(String.format("At %d: %s", s.getPosition(), s.getText()));
    }
}
```

[search(String, SearchOptions)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#search(java.lang.String,%20com.groupdocs.parser.options.SearchOptions)) method is used for the advanced search in PDF documents - like search with regular expressions, search by pages etc. [SearchOptions](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/SearchOptions) parameter is used to customize a search.

Here are the steps to search with a regular expression in PDF document:

*   Instantiate [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object for the initial document;
*   Instantiate [SearchOptions](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/SearchOptions) object with the parameters for the search;
*   Call [search(String, SearchOptions)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#search(java.lang.String,%20com.groupdocs.parser.options.SearchOptions)) method and obtain the collection of [SearchResult](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/SearchResult) objects;
*   Iterate through the collection and get the position and text.

The following example shows how to search with a regular expression in PDF document:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SamplePdf)) {
    // Search with a regular expression with case matching
    Iterable<SearchResult> sr = parser.search("(\\\\sut\\\\s)", new SearchOptions(true, false, true));
    // Iterate over search results
    for (SearchResult s : sr) {
        // Print an index and found text:
        System.out.println(String.format("At %d: %s", s.getPosition(), s.getText()));
    }
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).