---
id: extract-metadata-from-microsoft-office-word-documents
url: parser/java/extract-metadata-from-microsoft-office-word-documents
title: Extract metadata from Microsoft Office Word documents
weight: 2
description: "Learn how to extract metadata from Word documents (.doc, .docx) using GroupDocs.Parser for Java. Extract document properties like author, title, creation date, comments, and revision information from Word files."
keywords: "extract Word metadata, .doc metadata extraction, .docx metadata parser, Word document properties, document metadata API, Java Word parser, GroupDocs.Parser Word, extract document properties"
productName: GroupDocs.Parser for Java
hideChildren: False
---

## What is Word document metadata?

Word document metadata is hidden information stored inside your .doc and .docx files. It's like a "digital fingerprint" that contains details about the document such as:

- Who wrote the document
- When it was created and last modified
- How much time was spent writing it
- The document's title, subject, and keywords
- Which version of Word was used

You can see some of this information by right-clicking a Word file and selecting "Properties" - but with GroupDocs.Parser, you can extract this data programmatically using Java code.

## Why extract Word metadata?

Here are common scenarios where extracting Word metadata is useful:

**Document management:**
- Automatically organize documents by author or creation date
- Build searchable document libraries
- Track document versions and changes

**Business applications:**
- Find all documents created by a specific employee
- Monitor document editing activity
- Ensure proper document attribution

**Compliance and auditing:**
- Track document history for legal requirements
- Monitor sensitive document access
- Maintain document lifecycle records

## What information can you extract?

The [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method can extract these details from Word documents:

| Name | What it tells you |
| --- | --- |
| **title** | The document title |
| **subject** | What the document is about |
| **keywords** | Tags or keywords for the document |
| **comments** | Any comments added to the document |
| **content-status** | The content status of the document |
| **category** | The category of the document |
| **company** | The company name associated with the file |
| **manager** | The manager associated with the document |
| **author** | Who created the document |
| **last-author** | Who last saved the document |
| **hyperlink-base** | The base string used for evaluating relative hyperlinks in the document |
| **application** | Which application created the document |
| **application-version** | The version of the application that created it |
| **template** | Which Word template was used |
| **created-time** | When the document was first created |
| **last-saved-time** | When someone last saved changes |
| **last-printed-time** | When the document was last printed |
| **revision-number** | How many times the file has been revised |
| **total-editing-time** | How many minutes were spent editing |

Properties that are empty in the document are not included in the result.

## How to extract Word metadata

It's simple! Just follow these 3 steps:

### Step 1: Create a Parser

Instantiate the [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object for your Word document:

```java
try (Parser parser = new Parser("your-document.docx")) {
    // Your code goes here
}
```

### Step 2: Get the metadata

Call the [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method to obtain the collection of [MetadataItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/MetadataItem) objects:

```java
Iterable<MetadataItem> metadata = parser.getMetadata();
```

### Step 3: Read the results

Loop through the collection and print metadata names and values:

```java
for (MetadataItem item : metadata) {
    System.out.println(item.getName() + ": " + item.getValue());
}
```

### Complete working example

Here's the full code that extracts and displays all metadata from a Word document:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SampleDocx)) {
    // Extract metadata from the document
    Iterable<MetadataItem> metadata = parser.getMetadata();
    // Iterate over metadata items
    for (MetadataItem item : metadata) {
        // Print an item name and value
        System.out.println(String.format("%s: %s", item.getName(), item.getValue()));
    }
}
```

{{< alert style="warning" >}}[getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method returns null value if metadata extraction isn't supported for the document. For example, metadata extraction isn't supported for Zip archive. Therefore, for Zip archive [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method returns null. If Microsoft Office Word document has no metadata, [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method returns an empty collection.{{< /alert >}}

## Supported Word file formats

This works with all common Microsoft Word formats:

- **.doc** - Older Word documents (Word 97-2003)
- **.docx** - Newer Word documents (Word 2007 and later)
- **.dot** - Word document templates
- **.dotx** - Word template files

## Practical examples

### Example 1: Find documents by author

Let's say you want to find all Word documents in a folder that were created by a specific person:

```java
try (DirectoryStream<Path> documents = Files.newDirectoryStream(Paths.get("C:\\MyDocuments"), "*.docx")) {
    for (Path document : documents) {
        try (Parser parser = new Parser(document.toString())) {
            Iterable<MetadataItem> metadata = parser.getMetadata();
            if (metadata == null) {
                continue;
            }
            for (MetadataItem item : metadata) {
                if ("author".equals(item.getName()) && "John Smith".equals(item.getValue())) {
                    System.out.println("Found document by John Smith: " + document.getFileName());
                }
            }
        }
    }
}
```

### Example 2: Document summary report

Create a summary report showing key information about your Word documents:

```java
try (Parser parser = new Parser("report.docx")) {
    Iterable<MetadataItem> metadata = parser.getMetadata();

    System.out.println("=== Document Summary ===");
    System.out.println("Title: " + getValue(metadata, "title"));
    System.out.println("Author: " + getValue(metadata, "author"));
    System.out.println("Created: " + getValue(metadata, "created-time"));
    System.out.println("Last Modified: " + getValue(metadata, "last-saved-time"));
    System.out.println("Total Editing Time: " + getValue(metadata, "total-editing-time") + " minutes");
}

private static String getValue(Iterable<MetadataItem> metadata, String name) {
    if (metadata != null) {
        for (MetadataItem item : metadata) {
            if (name.equals(item.getName())) {
                return item.getValue();
            }
        }
    }
    return "Not available";
}
```

## Common use cases

**For businesses:**
- Track employee document creation and editing activity
- Organize documents by department or project
- Monitor document compliance and approval workflows

**For developers:**
- Build document management systems
- Create automated file organization tools
- Add document search and filtering capabilities

**For content managers:**
- Maintain document libraries and archives
- Track document versions and revision history
- Ensure proper document metadata standards

## Troubleshooting common issues

**Problem**: Not getting any metadata back?
- **Solution**: The document might not have metadata properties set, or the file format might not support metadata extraction (in this case `getMetadata` returns null)

**Problem**: Getting a "file not found" error?
- **Solution**: Check that the file path is correct and the file exists

**Problem**: Can't read password-protected documents?
- **Solution**: Pass the password with [LoadOptions](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/LoadOptions) when you create the `Parser` object. See [Password-protected documents]({{< ref "parser/java/developer-guide/advanced-usage/loading/password-protected-documents.md" >}}) for details

**Problem**: Some metadata fields are missing?
- **Solution**: This is normal - not all Word documents have all metadata fields populated, and empty fields are not returned

## Best practices

1. **Always use try-with-resources** to properly close Parser objects
2. **Check for null values** when the format may not support metadata extraction
3. **Handle exceptions** for corrupted or inaccessible files
4. **Test with different Word versions** to ensure compatibility
5. **Validate file formats** before attempting to extract metadata

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
