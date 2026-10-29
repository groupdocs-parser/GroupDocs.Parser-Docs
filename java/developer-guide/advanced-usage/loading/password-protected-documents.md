---
id: password-protected-documents
url: parser/java/password-protected-documents
title: Password-protected documents
weight: 4
description: "Learn how to open and process password-protected PDF and Office documents with GroupDocs.Parser for Java. Includes error handling examples and supported encryption types."
keywords: open the password-protected documents, encrypted PDF Java, password-protected Office files, LoadOptions password
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---
GroupDocs.Parser provides the functionality to open and process password-protected documents including encrypted PDFs and password-protected Office files (DOCX, XLSX, PPTX).

## Supported Encryption Types

The library supports the following encryption types:
- **PDF:** Standard password encryption (user password)
- **Office Documents:** Password-protected DOCX, XLSX, PPTX files
- **Legacy Office:** Password-protected DOC, XLS, PPT files

## Basic Usage

Here are the steps to work with password-protected documents:

1. Instantiate the [LoadOptions](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/LoadOptions) object and pass the password in the [LoadOptions(String)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/LoadOptions#LoadOptions(java.lang.String)) constructor;
2. Create [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object with `LoadOptions`;
3. Call any extraction method.

The following code sample shows how to process password-protected documents:

```java
try {
    String password = "123456";
    // Create an instance of Parser class with the password:
    try (Parser parser = new Parser(Constants.SamplePassword, new LoadOptions(password))) {
        // Check if text extraction is supported
        if (!parser.getFeatures().isText()) {
            System.out.println("Text extraction isn't supported.");
            return;
        }
        // Print the document text
        try (TextReader reader = parser.getText()) {
            System.out.println(reader.readToEnd());
        }
    }
} catch (InvalidPasswordException ex) {
    // Print the message if the password is incorrect or empty
    System.out.println("Invalid password");
}
```

## Exception Handling

When working with password-protected documents, the following exceptions may be thrown. All of them extend [GroupDocsParserException](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.exceptions/GroupDocsParserException), which is an unchecked exception (`RuntimeException`):

| Exception | When It's Thrown | Resolution |
|-----------|------------------|------------|
| [InvalidPasswordException](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.exceptions/InvalidPasswordException) | Password is incorrect, empty, or not provided | Provide the correct password in `LoadOptions` |
| [UnsupportedDocumentFormatException](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.exceptions/UnsupportedDocumentFormatException) | Document format is not recognized or not supported | Check if the file format is in the [supported formats list]({{< ref "parser/java/getting-started/supported-document-formats.md" >}}) |
| [CorruptedDocumentException](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.exceptions/CorruptedDocumentException) | Document is corrupted | Verify file integrity |
| [GroupDocsParserException](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.exceptions/GroupDocsParserException) | General parsing error | Check the exception message |

The following example handles these exceptions; catch the specific exceptions before `GroupDocsParserException`:

```java
try (Parser parser = new Parser(filePath, new LoadOptions(password))) {
    try (TextReader reader = parser.getText()) {
        System.out.println(reader == null ? "Text extraction isn't supported." : reader.readToEnd());
    }
} catch (InvalidPasswordException ex) {
    // Thrown when password is incorrect or empty
    System.out.println("Invalid password: " + ex.getMessage());
} catch (UnsupportedDocumentFormatException ex) {
    // Thrown when document format is not supported
    System.out.println("Unsupported format: " + ex.getMessage());
} catch (GroupDocsParserException ex) {
    // General parsing exception
    System.out.println("Parsing error: " + ex.getMessage());
}
```

## Check if Document is Password-Protected

Before attempting to open a document, you can check if it requires a password. The [getFileInfo](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getFileInfo(java.lang.String)) static method of [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) class returns a [FileInfo](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/FileInfo) object, and its [isEncrypted](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/FileInfo#isEncrypted()) method returns `true` if a password is required. `getFileInfo` accepts a file path or an `InputStream`:

```java
// Get a file info
FileInfo info = Parser.getFileInfo(filePath);
// Check if the document is encrypted/password-protected
if (info.isEncrypted()) {
    System.out.println("This document is password-protected. Password is required.");
    // Prompt user for password or load it from configuration
    String password = getPasswordFromUser();

    try (Parser parser = new Parser(filePath, new LoadOptions(password))) {
        // Process the document
    }
} else {
    System.out.println("Document is not password-protected.");
    try (Parser parser = new Parser(filePath)) {
        // Process the document without password
    }
}
```

{{< alert style="info" >}}
For a password-protected DOCX, XLSX or PPTX file, [getFileType](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/FileInfo#getFileType()) returns `FileType.Unknown`, because the format of an encrypted document cannot be read without the password. [getSize](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/FileInfo#getSize()) returns the file size. For PDF documents `isEncrypted()` returns `true` only for encrypted documents. Both behaviours apply since version 26.10.
{{< /alert >}}

## Complete Example: Extract Text from Password-Protected PDF

```java
public static void extractTextFromProtectedPdf(String filePath, String password) {
    try {
        // Check if password is needed
        FileInfo fileInfo = Parser.getFileInfo(filePath);

        LoadOptions loadOptions = fileInfo.isEncrypted()
                ? new LoadOptions(password)
                : new LoadOptions();

        try (Parser parser = new Parser(filePath, loadOptions)) {
            if (!parser.getFeatures().isText()) {
                System.out.println("Text extraction is not supported.");
                return;
            }

            try (TextReader reader = parser.getText()) {
                if (reader != null) {
                    String extractedText = reader.readToEnd();
                    System.out.println(String.format("Extracted text length: %d characters", extractedText.length()));
                }
            }
        }
    } catch (InvalidPasswordException ex) {
        System.out.println("Error: Invalid password provided.");
    } catch (Exception ex) {
        System.out.println("Error: " + ex.getMessage());
    }
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
