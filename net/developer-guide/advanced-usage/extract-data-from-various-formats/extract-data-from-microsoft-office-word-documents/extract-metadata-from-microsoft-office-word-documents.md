---
id: extract-metadata-from-microsoft-office-word-documents
url: parser/net/extract-metadata-from-microsoft-office-word-documents
title: Extract metadata from Microsoft Office Word documents
weight: 2
description: "Learn how to extract metadata from Word documents (.doc, .docx) using GroupDocs.Parser for .NET. Extract document properties like author, title, creation date, comments, and revision information from Word files."
keywords: "extract Word metadata, .doc metadata extraction, .docx metadata parser, Word document properties, document metadata API, C# Word parser, GroupDocs.Parser Word, extract document properties"
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---

## What is Word Document Metadata?

Word document metadata is hidden information stored inside your .doc and .docx files. It's like a "digital fingerprint" that contains details about the document such as:

- Who wrote the document
- When it was created and last modified  
- How much time was spent writing it
- The document's title, subject, and keywords
- Which version of Word was used

You can see some of this information by right-clicking a Word file and selecting "Properties" - but with GroupDocs.Parser, you can extract this data programmatically using C# code.

## Why Extract Word Metadata?

Here are common scenarios where extracting Word metadata is useful:

**Document Management:**
- Automatically organize documents by author or creation date
- Build searchable document libraries
- Track document versions and changes

**Business Applications:**
- Find all documents created by a specific employee
- Monitor document editing activity
- Ensure proper document attribution

**Compliance & Auditing:**
- Track document history for legal requirements
- Monitor sensitive document access
- Maintain document lifecycle records

## What Information Can You Extract?

The [GetMetadata](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getmetadata) method can extract these details from Word documents:

| Information | What it tells you |
| --- | --- |
| **title** | The document title |
| **author** | Who created the document |
| **subject** | What the document is about |
| **keywords** | Tags or keywords for the document |
| **comments** | Any comments added to the document |
| **company** | The company name associated with the file |
| **manager** | The manager associated with the document |
| **created-time** | When the document was first created |
| **last-saved-time** | When someone last saved changes |
| **last-printed-time** | When the document was last printed |
| **total-editing-time** | How many minutes were spent editing |
| **revision-number** | How many times the file has been revised |
| **template** | Which Word template was used |
| **application** | Which application created the document |
| **application-version** | The version of Word that created it |

*Plus additional technical details like hyperlink-base, content-status, category, and last-author.*

## How to Extract Word Metadata

It's simple! Just follow these 3 easy steps:

### Step 1: Create a Parser
Point the Parser to your Word document:

```csharp
using(Parser parser = new Parser("your-document.docx"))
{
    // Your code goes here
}
```

### Step 2: Get the Metadata  
Extract all the metadata information:

```csharp
IEnumerable<MetadataItem> metadata = parser.GetMetadata();
```

### Step 3: Read the Results
Loop through and display what you found:

```csharp
foreach(MetadataItem item in metadata)
{
    Console.WriteLine($"{item.Name}: {item.Value}");
}
```

### Complete Working Example

Here's the full code that extracts and displays all metadata from a Word document:

```csharp
// Create an instance of Parser class
using(Parser parser = new Parser(filePath))
{
    // Extract metadata from the document
    IEnumerable<MetadataItem> metadata = parser.GetMetadata();
    
    // Iterate over metadata items
    foreach(MetadataItem item in metadata)
    {
        // Print the item name and value
        Console.WriteLine(string.Format("{0}: {1}", item.Name, item.Value));
    }
}
```

{{< alert style="warning" >}}
**Note**: If a Word document doesn't have metadata or the file format isn't supported, you might get no results. This is normal - not all documents have complete metadata information.
{{< /alert >}}

## Supported Word File Formats

This works with all common Microsoft Word formats:
- **.doc** - Older Word documents (Word 97-2003)
- **.docx** - Newer Word documents (Word 2007 and later)
- **.dot** - Word document templates
- **.dotx** - Word template files

## Practical Examples

### Example 1: Find Documents by Author

Let's say you want to find all Word documents in a folder that were created by a specific person:

```csharp
string[] documents = Directory.GetFiles(@"C:\MyDocuments", "*.docx");

foreach (string document in documents)
{
    using (Parser parser = new Parser(document))
    {
        var metadata = parser.GetMetadata();
        var author = metadata.FirstOrDefault(m => m.Name == "author");
        
        if (author?.Value?.ToString() == "John Smith")
        {
            Console.WriteLine($"Found document by John Smith: {Path.GetFileName(document)}");
        }
    }
}
```

### Example 2: Document Summary Report

Create a summary report showing key information about your Word documents:

```csharp
using (Parser parser = new Parser("report.docx"))
{
    var metadata = parser.GetMetadata();
    
    Console.WriteLine("=== Document Summary ===");
    Console.WriteLine($"Title: {GetValue(metadata, "title")}");
    Console.WriteLine($"Author: {GetValue(metadata, "author")}");
    Console.WriteLine($"Created: {GetValue(metadata, "created-time")}");
    Console.WriteLine($"Last Modified: {GetValue(metadata, "last-saved-time")}");
    Console.WriteLine($"Total Editing Time: {GetValue(metadata, "total-editing-time")} minutes");
}

string GetValue(IEnumerable<MetadataItem> metadata, string name)
{
    return metadata.FirstOrDefault(m => m.Name == name)?.Value?.ToString() ?? "Not available";
}
```

## Common Use Cases

**For Businesses:**
- Track employee document creation and editing activity
- Organize documents by department or project
- Monitor document compliance and approval workflows

**For Developers:**
- Build document management systems
- Create automated file organization tools
- Add document search and filtering capabilities

**For Content Managers:**
- Maintain document libraries and archives
- Track document versions and revision history
- Ensure proper document metadata standards

## Troubleshooting Common Issues

**Problem**: Not getting any metadata back?
- **Solution**: The document might not have metadata properties set, or the file format might not be supported

**Problem**: Getting a "file not found" error?
- **Solution**: Check that the file path is correct and the file exists

**Problem**: Can't read password-protected documents?
- **Solution**: Remove password protection first, as encrypted documents require special handling

**Problem**: Some metadata fields are empty?
- **Solution**: This is normal - not all Word documents have all metadata fields populated

## Best Practices

1. **Always use `using` statements** to properly dispose of Parser objects
2. **Check for null values** when accessing specific metadata fields
3. **Handle exceptions** for corrupted or inaccessible files
4. **Test with different Word versions** to ensure compatibility
5. **Validate file formats** before attempting to extract metadata

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).