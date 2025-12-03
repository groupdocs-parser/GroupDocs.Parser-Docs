---
id: extract-text-from-microsoft-office-word-documents
url: parser/net/extract-text-from-microsoft-office-word-documents
title: Extract text from Microsoft Office Word documents
weight: 1
version: 23.5
description: "Learn how to extract text from Word documents (.doc, .docx) using GroupDocs.Parser for .NET. Extract text from entire documents or specific pages with error handling in C#."
keywords: extract text, extract text from Microsoft Office Word, .doc, .docx, extract text from DOCX, Word text extraction C#, DOC parser
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
tags: csharp, parser, word, docx, doc, text-extraction, v23.5
---
To extract a text from Microsoft Office Word documents [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) and [GetText(pageIndex)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/2) methods are used. These methods allow to extract a text from the entire document or a text from the selected page. [TextOptions](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/textoptions) parameter is ignored for Microsoft Office Words documents.

Here are the steps to extract a text from Microsoft Office Word document:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Call [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object;
*   Read a text from *reader*.

{{< alert style="warning" >}}
[GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method returns *null* value if text extraction isn't supported for the document. For example, text extraction isn't supported for Zip archive. Therefore, for Zip archive [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method returns *null*. For empty Microsoft Office Word document [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method returns an empty [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object ([reader.ReadToEnd](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader.readtoend?view=netframework-2.0) method returns an empty string).
{{< /alert >}}

The following example demonstrates how to extract text from a Word document with error handling:

```csharp
using GroupDocs.Parser;
using GroupDocs.Parser.Exceptions;
using System;
using System.IO;

try
{
    // Create an instance of Parser class
    using (Parser parser = new Parser(filePath))
    {
        // Extract text into the reader
        using (TextReader reader = parser.GetText())
        {
            // Check if text extraction is supported
            if (reader == null)
            {
                Console.WriteLine("Text extraction isn't supported for this Word document.");
                return;
            }
            
            // Read and print the text
            string extractedText = reader.ReadToEnd();
            
            if (string.IsNullOrEmpty(extractedText))
            {
                Console.WriteLine("The Word document appears to be empty or contains no extractable text.");
            }
            else
            {
                Console.WriteLine($"Extracted {extractedText.Length} characters of text:");
                Console.WriteLine(extractedText);
            }
        }
    }
}
catch (FileNotFoundException)
{
    Console.WriteLine($"Error: Word document not found at path '{filePath}'");
}
catch (InvalidPasswordException)
{
    Console.WriteLine("Error: The Word document is password-protected. Use LoadOptions to provide the password.");
}
catch (ParserException ex)
{
    Console.WriteLine($"Error extracting text from Word document: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Unexpected error: {ex.Message}");
}
```

Here are the steps to extract a text from the page of Microsoft Office Word document:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Call [GetDocumentInfo](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getdocumentinfo) method and obtain [IDocumentInfo](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo) object with [page count](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/pagecount);
*   Call [GetText(pageIndex)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/2) method with the page index and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object;
*   Read a text from *reader*.

{{< alert style="warning" >}}
Text extraction from Microsoft Office Word document page is more resource consuming operation. Use text extraction from the entire document where it is applicable.
{{< /alert >}}

The following example demonstrates how to extract a text from the page of Microsoft Office Word document:

```csharp
// Create an instance of Parser class
using(Parser parser = new Parser(filePath))
{
    // Get the document info
    IDocumentInfo documentInfo = parser.GetDocumentInfo();
  
    // Iterate over pages
    for(int p = 0; p < documentInfo.PageCount; p++)
    {
        // Print a page number 
        Console.WriteLine(string.Format("Page {0}/{1}", p + 1, documentInfo.PageCount));
  
        // Extract a text into the reader
        using(TextReader reader = parser.GetText(p))
        {
            // Print a text from the document
            Console.WriteLine(reader.ReadToEnd());
        }
    }
}
```

GroupDocs.Parser also allows to extract a text from Microsoft Office Word documents as HTML, Markdown and formatted plain text. For more details, see [Extract Formatted Text]({{< ref "parser/net/developer-guide/advanced-usage/working-with-text/working-with-formatted-text/extract-formatted-text-from-document.md" >}}).

Here are the steps to extract a text from Microsoft Office Word document as HTML:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Call [GetFormattedText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getformattedtext) method and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object;
*   Read a text from *reader*.

The following example shows how to extract a text from Microsoft Office Word document as HTML:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser(filePath))
{
    // Extract a formatted text into the reader
    using (TextReader reader = parser.GetFormattedText(new FormattedTextOptions(FormattedTextMode.Html)))
    {
        // Print a formatted text from the document
        Console.WriteLine(reader.ReadToEnd());
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