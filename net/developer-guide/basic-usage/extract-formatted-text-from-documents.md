---
id: extract-formatted-text-from-documents
url: parser/net/extract-formatted-text-from-documents
title: Extract formatted text from documents
weight: 6
version: 23.5
description: "Learn how to extract formatted text as HTML, Markdown, or PlainText from PDF, Word, Excel, and 50+ document formats using GroupDocs.Parser for .NET. Preserve formatting with C# code examples."
keywords: Extract formatted text, Extract data, HTML text extraction, Markdown extraction, EPUB, FB2, CHM, DOC, DOCX, PPT, PPTX, XLS, XLSX, formatted text C#
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
tags: csharp, parser, formatted-text, html, markdown, v23.5
---
GroupDocs.Parser allows to extract formatted text from documents for those cases when simple plain text is not enough and you may need to keep formatting like text style, table layout etc.

This feature allows to extract text with integrated HTML tags, or Markdown syntax. Even PlainText mode allows to convert your documents to high quality text with integrated ASCII formatting symbols for tables, lists etc.

## Extract formatted text from documents

To extract a formatted text from documents simply call the [GetFormattedText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getformattedtext) method:

```csharp
TextReader GetFormattedText(FormattedTextOptions options);

```

Methods return an instance of [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) class with an extracted text. [FormattedTextOptions](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/formattedtextoptions) has the following constructor:

```csharp
FormattedTextOptions(FormattedTextMode mode)

```

[FormattedTextMode](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/formattedtextmode) enumeration has the following members:

| Member | Description |
| --- | --- |
| Html | Html format. |
| Markdown | Markdown format. |
| PlainText | Plain text format. |

Here are the steps to extract a HTML formatted text from the document:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Instantiate [FormattedTextOptions](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/formattedtextoptions) with HTML text mode;
*   Call [GetFormattedText](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getformattedtext) method and obtain [TextReader](https://docs.microsoft.com/en-us/dotnet/api/system.io.textreader?view=netframework-2.0) object;
*   Check if *reader* isn't *null* (formatted text extraction is supported for the document);
*   Read a text from *reader*.

The following example shows how to extract document text as HTML with error handling:

```csharp
using GroupDocs.Parser;
using GroupDocs.Parser.Options;
using GroupDocs.Parser.Exceptions;
using System;
using System.IO;

try
{
    // Create an instance of Parser class
    using (Parser parser = new Parser(filePath))
    {
        // Extract formatted text as HTML
        using (TextReader reader = parser.GetFormattedText(new FormattedTextOptions(FormattedTextMode.Html)))
        {
            // Check if formatted text extraction is supported
            if (reader == null)
            {
                Console.WriteLine("Formatted text extraction isn't supported for this document format.");
                return;
            }
            
            // Read and print the formatted text
            string htmlText = reader.ReadToEnd();
            Console.WriteLine(htmlText);
        }
    }
}
catch (FileNotFoundException)
{
    Console.WriteLine($"Error: File not found at path '{filePath}'");
}
catch (ParserException ex)
{
    Console.WriteLine($"Error extracting formatted text: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Unexpected error: {ex.Message}");
}
```

### Example: Extract Text as Markdown

```csharp
using GroupDocs.Parser;
using GroupDocs.Parser.Options;

using (Parser parser = new Parser(filePath))
{
    // Extract formatted text as Markdown
    using (TextReader reader = parser.GetFormattedText(new FormattedTextOptions(FormattedTextMode.Markdown)))
    {
        if (reader != null)
        {
            string markdownText = reader.ReadToEnd();
            Console.WriteLine(markdownText);
        }
        else
        {
            Console.WriteLine("Markdown extraction is not supported for this format.");
        }
    }
}
```

### Example: Extract Formatted Plain Text

```csharp
using GroupDocs.Parser;
using GroupDocs.Parser.Options;

using (Parser parser = new Parser(filePath))
{
    // Extract formatted text as PlainText (with ASCII formatting symbols)
    using (TextReader reader = parser.GetFormattedText(new FormattedTextOptions(FormattedTextMode.PlainText)))
    {
        if (reader != null)
        {
            string formattedPlainText = reader.ReadToEnd();
            Console.WriteLine(formattedPlainText);
        }
    }
}
```

## More resources

### Advanced usage topics

To learn more about text extraction features, please refer the [advanced help section]({{< ref "parser/net/developer-guide/advanced-usage/working-with-text/working-with-formatted-text/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online text extractor App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to extract text from your documents with our free online [GroupDocs Parser App](https://products.groupdocs.app/parser).