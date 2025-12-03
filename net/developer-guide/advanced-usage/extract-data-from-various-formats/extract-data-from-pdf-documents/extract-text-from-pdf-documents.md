---
id: extract-text-from-pdf-documents
url: parser/net/extract-text-from-pdf-documents
title: Extract text from PDF documents
weight: 1
version: 23.5
description: "Learn how to extract text from PDF documents using GroupDocs.Parser for .NET. Extract text from entire PDF or specific pages with error handling. Includes PDF text extraction library C# examples."
keywords: extract text, extract text from PDF documents, extract text from PDF C#, PDF text extraction library C#, PDF parser C#, read PDF text
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
tags: csharp, parser, pdf, text-extraction, v23.5
---
To extract a text from PDF documents [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) and [GetText(pageIndex)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/2) method is used. These methods allow to extract a text from the entire document or a text from the selected page.

Here are the steps to extract a text from PDF document:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Call [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object;
*   Read a text from *reader*.

{{< alert style="warning" >}}
[GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method returns *null* value if text extraction isn't supported for the document. For example, text extraction isn't supported for Zip archive. Therefore, for Zip archive [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method returns *null*. For empty PDF document [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method returns an empty [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object ([reader.ReadToEnd](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader.readtoend?view=netframework-2.0) method returns an empty string).
{{< /alert >}}

The following example demonstrates how to extract text from a PDF document with error handling:

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
                Console.WriteLine("Text extraction isn't supported for this PDF file.");
                return;
            }
            
            // Read and print the text
            string extractedText = reader.ReadToEnd();
            
            if (string.IsNullOrEmpty(extractedText))
            {
                Console.WriteLine("The PDF document appears to be empty or contains no extractable text.");
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
    Console.WriteLine($"Error: PDF file not found at path '{filePath}'");
}
catch (InvalidPasswordException)
{
    Console.WriteLine("Error: The PDF is password-protected. Use LoadOptions to provide the password.");
}
catch (ParserException ex)
{
    Console.WriteLine($"Error extracting text from PDF: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Unexpected error: {ex.Message}");
}
```

Here are the steps to extract a text from the page of PDF document:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Call [GetDocumentInfo](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getdocumentinfo) method and obtain [IDocumentInfo](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo) object with [page count](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/pagecount);
*   Call [GetText(pageIndex)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/2) method with the page index and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object;
*   Read a text from *reader*.

The following example demonstrates how to extract a text from the page of PDF document:

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

Raw mode allows to increase the speed of text extraction due to poor formatting accuracy. [GetText(TextOptions)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/1) and [GetText(pageIndex, TextOptions)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/3) methods are used to extract a text in raw mode.

{{< alert style="warning" >}}
Some documents may have different page numbers in raw and accurate modes. Use [DocumentInfo.RawPageCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/rawpagecount) instead of [IDocumentInfo.PageCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/pagecount) in raw mode.
{{< /alert >}}

Here are the steps to extract a raw text from the page of PDF document:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Instantiate [TextOptions](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/textoptions) object with *true* parameter;
*   Call [GetDocumentInfo](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getdocumentinfo);
*   Use [RawPageCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/rawpagecount) instead of [PageCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/idocumentinfo/properties/pagecount) to avoid extra calculations;
*   Call [GetText(pageIndex, TextOptions)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/3) method with the sheet index and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object;
*   Read a text from *reader*.

The following example demonstrates how to extract a raw text from the page of PDF document:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser(filePath))
{
    // Get the document info
    IDocumentInfo documentInfo = parser.GetDocumentInfo();
    // Check if the document has pages
    if (documentInfo == null || documentInfo.RawPageCount == 0)
    {
        Console.WriteLine("Document hasn't pages.");
        return;
    }
    // Iterate over pages
    for (int p = 0; p < documentInfo.RawPageCount; p++)
    {
        // Print a pagenumber 
        Console.WriteLine(string.Format("Slide {0}/{1}", p + 1, documentInfo.RawPageCount));
        // Extract a text into the reader
        using (TextReader reader = parser.GetText(p, new TextOptions(true)))
        {
            // Print a text from the document page
            Console.WriteLine(reader.ReadToEnd());
        }
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