---
id: search-text-in-html-documents
url: parser/net/search-text-in-html-documents
title: Search text in HTML documents
weight: 2
description: "To search a keyword in HTML documents Search(String) method is used. This method returns the collection of SearchResult objects."
keywords: search a keyword, search a keyword in HTML
productName: GroupDocs.Parser for .NET
hideChildren: False
---
To search a keyword in HTML documents [Search(String)](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/search) method is used. This method returns the collection of [SearchResult](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/searchresult) objects. For details, see [Search Text]({{< ref "parser/net/developer-guide/advanced-usage/working-with-text/search-text.md" >}}).

Here are the steps to search a keyword in HTML document:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Call [Search(string)](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/search) method and obtain the collection of [SearchResult](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/searchresult) objects;
*   Iterate through the collection and get the position and text.

{{< alert style="warning" >}}
[Search(String)](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/search) method returns *null* value if search isn't supported for the document. For example, text extraction isn't supported for Zip archive. Therefore, for Zip archive [Search(String)](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/search) method returns *null*. For empty HTML document [Search(String)](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/search) method returns an empty collection.
{{< /alert >}}

The following example shows how to find a keyword in HTML document:

```csharp
// Create an instance of Parser class
using(Parser parser = new Parser(filePath))
{
    // Search a keyword:
    IEnumerable<SearchResult> sr = parser.Search("page number");
   
    // Iterate over search results
    foreach(SearchResult s in sr)
    {
        // Print an index and found text:
        Console.WriteLine(string.Format("At {0}: {1}", s.Position, s.Text));
    }
}
```

[Search(String, SearchOptions)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/search/methods/1) is used for the advanced search in HTML documents - like search with regular expressions. [SearchOptions](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/searchoptions) parameter is used to customize a search.

Here are the steps to search with a regular expression in HTML document:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Instantiate [SearchOptions](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/searchoptions) object with the parameters for the search;
*   Call [Search(string, SearchOptions)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/search/methods/1) method and obtain the collection of [SearchResult](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/searchresult) objects;
*   Iterate through the collection and get the position and text.

The following example shows how to search with a regular expression in HTML document:

```csharp
// Create an instance of Parser class
using(Parser parser = new Parser(filePath))
{
    // Search with a regular expression with case matching
    IEnumerable<SearchResult> sr = parser.Search("page number: [0-9]+", new SearchOptions(true, false, true));
    // Iterate over search results
    foreach(SearchResult s in sr)
    {
        // Print an index and found text:
        Console.WriteLine(string.Format("At {0}: {1}", s.Position, s.Text));
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