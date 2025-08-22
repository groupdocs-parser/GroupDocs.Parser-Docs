---
id: extract-metadata-from-microsoft-office-powerpoint-presentations
url: parser/net/extract-metadata-from-microsoft-office-powerpoint-presentations
title: Extract metadata from Microsoft Office PowerPoint presentations
weight: 2
description: "Learn how to extract metadata from PowerPoint presentations (.ppt, .pptx) using GroupDocs.Parser for .NET. Extract document properties like author, title, creation date, and comments from presentation files."
keywords: "extract PowerPoint metadata, .ppt metadata extraction, .pptx metadata parser, PowerPoint document properties, presentation metadata API, C# PowerPoint parser"
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---

## What is PowerPoint Metadata?

PowerPoint metadata is like a "digital label" attached to your presentation files. It contains information about the file such as:

- Who created it
- When it was made
- The title and subject
- How long someone spent editing it
- Which version of PowerPoint was used

Think of it like the "Properties" you see when you right-click on a file - but now you can extract this information programmatically using code.

## Why Extract PowerPoint Metadata?

Here are some common reasons developers extract metadata from PowerPoint files:

- **File Organization**: Automatically sort presentations by author, date, or company
- **Document Tracking**: Find out who last modified a presentation and when
- **Content Management**: Build searchable libraries of presentations
- **Compliance**: Track document history for auditing purposes

## Available Metadata Information

When you extract metadata from a PowerPoint presentation using GroupDocs.Parser, you can get these details:

| Information | What it tells you |
| --- | --- |
| **title** | The presentation title |
| **author** | Who created the presentation |
| **subject** | What the presentation is about |
| **keywords** | Tags or keywords for the presentation |
| **comments** | Any comments added to the presentation |
| **company** | The company name associated with the file |
| **created-time** | When the presentation was first created |
| **last-saved-time** | When someone last saved changes |
| **total-editing-time** | How many minutes were spent editing |
| **revision-number** | How many times the file has been revised |

*And several other technical details...*

## How to Extract the Metadata

It's really simple! Just follow these 3 steps:

### Step 1: Create a Parser
First, create a Parser object and point it to your PowerPoint file:

```csharp
using(Parser parser = new Parser("your-presentation.pptx"))
{
    // Your code goes here
}
```

### Step 2: Get the Metadata
Ask the parser to extract all the metadata:

```csharp
IEnumerable<MetadataItem> metadata = parser.GetMetadata();
```

### Step 3: Read the Information
Loop through the results and print out what you found:

```csharp
foreach(MetadataItem item in metadata)
{
    Console.WriteLine($"{item.Name}: {item.Value}");
}
```

### Complete Example

Here's the full working example:

```csharp
// Create an instance of Parser class
using(Parser parser = new Parser(filePath))
{
    // Extract metadata from the presentation
    IEnumerable<MetadataItem> metadata = parser.GetMetadata();
  
    // Iterate over metadata items
    foreach(MetadataItem item in metadata)
    {
        // Print the item name and value
        Console.WriteLine(string.Format("{0}: {1}", item.Name, item.Value));
    }
}
```

## What File Types Work?

This works with all common PowerPoint formats:
- **.ppt** - Older PowerPoint files
- **.pptx** - Newer PowerPoint files  
- **.pps** - PowerPoint slideshow files
- **.ppsx** - Newer slideshow files

## Important Notes

{{< alert style="warning" >}}
**Remember**: If a PowerPoint file doesn't have metadata or the file type isn't supported, you might get no results. This is normal - not all files have metadata information.
{{< /alert >}}

## Real-World Example

Let's say you have a folder full of PowerPoint presentations and you want to find out who created each one:

```csharp
string[] presentations = Directory.GetFiles(@"C:\MyPresentations", "*.pptx");

foreach (string presentation in presentations)
{
    using (Parser parser = new Parser(presentation))
    {
        var metadata = parser.GetMetadata();
        var author = metadata.FirstOrDefault(m => m.Name == "author");
        
        Console.WriteLine($"File: {Path.GetFileName(presentation)}");
        Console.WriteLine($"Author: {author?.Value ?? "Unknown"}");
        Console.WriteLine("---");
    }
}
```

This will show you the author of each PowerPoint file in your folder!

## Common Use Cases

**For Businesses:**
- Track which employee created which presentation
- Find presentations by topic or keyword
- Monitor when presentations were last updated

**For Developers:**
- Build document management systems
- Create automated file organizing tools
- Add search functionality to presentation libraries

**For Content Managers:**
- Organize presentation libraries
- Track document versions and changes
- Ensure proper attribution and ownership

## Troubleshooting

**Problem**: Getting no metadata back?
- **Solution**: The file might not have metadata, or the file format isn't supported

**Problem**: Can't read the file?
- **Solution**: Make sure the file path is correct and the file isn't password-protected

**Problem**: Getting an error?
- **Solution**: Check that the file isn't corrupted and is a valid PowerPoint file

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).