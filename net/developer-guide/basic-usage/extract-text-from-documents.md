---
id: extract-text-from-documents
url: parser/net/extract-text-from-documents
title: Extract text from documents
weight: 5
version: 23.5
description: "Learn how to extract text from PDF, Word, Excel, PowerPoint, and 50+ document formats using GroupDocs.Parser for .NET. Simple C# code examples for extract text from PDF C# scenarios."
keywords: Extract text, extract text from PDF C#, PDF text extraction library C#, EPUB, FB2, CHM, pdf, DOC, DOCX, PPT, PPTX, XLS, XLSX, text extraction C#
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
tags: csharp, parser, text-extraction, v23.5
---
GroupDocs.Parser allows to extract text from PDF, Emails, Ebooks, Microsoft Office formats: Word (DOC, DOCX), PowerPoint (PPT, PPTX), Excel (XLS, XLSX), LibreOffice formats and many others (see full list at [supported document formats]({{< ref "parser/net/getting-started/supported-document-formats.md" >}}) article).

GroupDocs.Parser's text extractor is easy to use and powerful at the same time (to resolve complex scenarios see [advanced usage section]({{< ref "parser/net/developer-guide/advanced-usage/working-with-text/_index.md" >}})).

This article demonstrates how to implement the simplest scenario - extract text from any supported format without additional settings.

## Extract text from documents

To extract text from documents simply call [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method:

```csharp
TextReader GetText();


```

Methods return an instance of [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) class with an extracted text. 

Here are the steps to extract a text from the document:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Call [GetText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettext) method and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object;
*   Check if *reader* isn't *null* (text extraction is supported for the document);
*   Read a text from *reader*.

The following example shows how to extract text from a document with error handling:

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
            // Check if text extraction is supported for this document format
            if (reader == null)
            {
                Console.WriteLine("Text extraction isn't supported for this document format.");
                return;
            }
            
            // Read and print the extracted text
            string extractedText = reader.ReadToEnd();
            Console.WriteLine(extractedText);
        }
    }
}
catch (FileNotFoundException)
{
    Console.WriteLine($"Error: File not found at path '{filePath}'");
}
catch (ParserException ex)
{
    Console.WriteLine($"Error extracting text: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Unexpected error: {ex.Message}");
}
```

### Example: Extract Text with Null Check

```csharp
using GroupDocs.Parser;

using (Parser parser = new Parser(filePath))
{
    using (TextReader reader = parser.GetText())
    {
        if (reader != null)
        {
            string text = reader.ReadToEnd();
            if (string.IsNullOrEmpty(text))
            {
                Console.WriteLine("Document appears to be empty.");
            }
            else
            {
                Console.WriteLine($"Extracted {text.Length} characters of text.");
                Console.WriteLine(text);
            }
        }
        else
        {
            Console.WriteLine("Text extraction is not supported for this file format.");
        }
    }
}
```

## More resources

### Advanced usage topics

To learn more about text extraction features, please refer the [advanced usage section]({{< ref "parser/net/developer-guide/advanced-usage/working-with-text/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online text extractor App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to extract text from your documents with our free online [GroupDocs Parser App](https://products.groupdocs.app/parser).