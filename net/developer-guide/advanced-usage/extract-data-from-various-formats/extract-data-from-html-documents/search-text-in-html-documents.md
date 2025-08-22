
id: search-text-in-html-documents
url: parser/net/search-text-in-html-documents
title: Search text in HTML documents
weight: 2
description: "To search a keyword in HTML documents Search(String) method is used. This method returns the collection of SearchResult objects."
keywords: search a keyword, search a keyword in HTML
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true

To search a keyword in HTML documents [Search(String)](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/search) method is used. This method returns the collection of [SearchResult](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/searchresult) objects. For details, see [Search Text]({{< ref "parser/net/developer-guide/advanced-usage/working-with-text/search-text.md" >}}).

{{< alert style="warning" >}}
Parsing does not execute JavaScript or fetch external content; for dynamically generated HTML, pre-render or extract the actual text content before searching.
{{< /alert >}}

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

### Null vs. Empty Collection Results

The [Search()](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/search/methods/1) method behaves differently depending on the document type and its text content:

- **Returns `null`** → when the document type does not support searching (e.g., ZIP archives, images, or other non-text formats).  
- **Returns an empty collection** → when the document supports searching (like HTML) but contains no searchable text.  


### Example 1: Search in a non-searchable document (returns `null`)
```csharp
using (Parser parser = new Parser("sample.zip")) // ZIP archives are not searchable
{
    var results = parser.Search("example");    
    
    if (results == null)
        Console.WriteLine("Search returned null: document type does not support searching.");
}
```

### Example 2: Search in an empty HTML file (returns empty collection)
```csharp
using (Parser parser = new Parser("empty.html")) // Empty HTML without text
{
    var results = parser.Search("example");
    if (results != null && results.Count == 0)
        Console.WriteLine("Search returned empty collection: no text found in the document.");
}
```

### Example 3: Search in a valid HTML file (returns results)
```csharp
using (Parser parser = new Parser("sample.html")) // Contains text
{
    var results = parser.Search("example");
    if (results != null)
    {
        foreach (var result in results)
            Console.WriteLine($"Found: '{result.Text}' at position {result.Position}");
    }
}
```

## Preprocessing Recommendations

GroupDocs.Parser works with the **raw text content** of HTML documents.  
It does **not** execute JavaScript or load external resources (such as CSS, images, or scripts).  
If your HTML file relies on JavaScript to generate content dynamically, the parser may return an empty collection.

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


## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).