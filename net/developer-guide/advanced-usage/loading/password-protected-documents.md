---
id: password-protected-documents
url: parser/net/password-protected-documents
title: Password-protected documents
weight: 2
version: 23.5
description: "Learn how to open and process password-protected PDF and Office documents. Includes error handling examples and supported encryption types."
keywords: open the password-protected documents, encrypted PDF C#, password-protected Office files, LoadOptions password
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
tags: csharp, parser, password-protected, encrypted-documents, v23.5
---
GroupDocs.Parser provides the functionality to open and process password-protected documents including encrypted PDFs and password-protected Office files (DOCX, XLSX, PPTX).

## Supported Encryption Types

The library supports the following encryption types:
- **PDF:** Standard password encryption (user password)
- **Office Documents:** Password-protected DOCX, XLSX, PPTX files
- **Legacy Office:** Password-protected DOC, XLS, PPT files

{{< alert style="warning" >}}
**Note:** The library cannot decrypt documents with owner passwords (restrictions passwords) that only restrict printing/editing. Only documents with user passwords (opening passwords) are supported.
{{< /alert >}}

## Basic Usage

Here are the steps to work with password-protected documents:

1. Instantiate the [LoadOptions](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/loadoptions) object and pass the password in the [LoadOptions(String)](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/loadoptions/constructors/4) constructor
2. Create [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object with LoadOptions
3. Call any extraction method

The following code sample shows how to process password-protected documents with comprehensive error handling:

```csharp
using GroupDocs.Parser;
using GroupDocs.Parser.Options;
using GroupDocs.Parser.Exceptions;
using System;

try
{
    string password = "your-password";
    
    // Create LoadOptions with password
    LoadOptions loadOptions = new LoadOptions(password);
    
    // Create an instance of Parser class with the password
    using (Parser parser = new Parser(filePath, loadOptions))
    {
        // Check if text extraction is supported
        if (!parser.Features.Text)
        {
            Console.WriteLine("Text extraction isn't supported for this format.");
            return;
        }
        
        // Print the document text
        using (TextReader reader = parser.GetText())
        {
            if (reader != null)
            {
                Console.WriteLine(reader.ReadToEnd());
            }
            else
            {
                Console.WriteLine("Text extraction returned null.");
            }
        }
    }
}
catch (InvalidPasswordException ex)
{
    // Thrown when password is incorrect or empty
    Console.WriteLine($"Invalid password: {ex.Message}");
}
catch (UnsupportedDocumentFormatException ex)
{
    // Thrown when document format is not supported
    Console.WriteLine($"Unsupported format: {ex.Message}");
}
catch (ParserException ex)
{
    // General parsing exception
    Console.WriteLine($"Parsing error: {ex.Message}");
}
```

## Exception Handling

When working with password-protected documents, the following exceptions may be thrown:

| Exception | When It's Thrown | Resolution |
|-----------|------------------|------------|
| [InvalidPasswordException](https://reference.groupdocs.com/net/parser/groupdocs.parser.exceptions/invalidpasswordexception) | Password is incorrect, empty, or not provided | Provide the correct password in LoadOptions |
| [UnsupportedDocumentFormatException](https://reference.groupdocs.com/net/parser/groupdocs.parser.exceptions/unsupporteddocumentformatexception) | Document format is not supported | Check if the file format is in the [supported formats list]({{< ref "parser/net/getting-started/supported-document-formats.md" >}}) |
| [ParserException](https://reference.groupdocs.com/net/parser/groupdocs.parser.exceptions/parserexception) | General parsing error (corrupted file, insufficient permissions, etc.) | Verify file integrity and access permissions |

## Check if Document is Password-Protected

Before attempting to open a document, you can check if it requires a password:

```csharp
using GroupDocs.Parser.Options;

// Get file information
FileInfo info = Parser.GetFileInfo(document);

// Check if the document is encrypted/password-protected
if (info.IsEncrypted)
{
    Console.WriteLine("This document is password-protected. Password is required.");
    // Prompt user for password or load from configuration
    string password = GetPasswordFromUser();
    
    LoadOptions loadOptions = new LoadOptions(password);
    using (Parser parser = new Parser(document, loadOptions))
    {
        // Process the document
    }
}
else
{
    Console.WriteLine("Document is not password-protected.");
    using (Parser parser = new Parser(document))
    {
        // Process the document without password
    }
}
```

## Complete Example: Extract Text from Password-Protected PDF

```csharp
using GroupDocs.Parser;
using GroupDocs.Parser.Options;
using GroupDocs.Parser.Exceptions;

public void ExtractTextFromProtectedPdf(string filePath, string password)
{
    try
    {
        // Check if password is needed
        FileInfo fileInfo = Parser.GetFileInfo(filePath);
        
        LoadOptions loadOptions = fileInfo.IsEncrypted 
            ? new LoadOptions(password) 
            : new LoadOptions();
        
        using (Parser parser = new Parser(filePath, loadOptions))
        {
            if (!parser.Features.Text)
            {
                Console.WriteLine("Text extraction is not supported.");
                return;
            }
            
            using (TextReader reader = parser.GetText())
            {
                if (reader != null)
                {
                    string extractedText = reader.ReadToEnd();
                    Console.WriteLine($"Extracted text length: {extractedText.Length} characters");
                }
            }
        }
    }
    catch (InvalidPasswordException)
    {
        Console.WriteLine("Error: Invalid password provided.");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error: {ex.Message}");
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