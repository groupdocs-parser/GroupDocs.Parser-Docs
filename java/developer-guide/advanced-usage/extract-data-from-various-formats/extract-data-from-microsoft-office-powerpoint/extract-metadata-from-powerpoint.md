---
id: extract-metadata-from-microsoft-office-powerpoint-presentations
url: parser/java/extract-metadata-from-microsoft-office-powerpoint-presentations
title: Extract metadata from Microsoft Office PowerPoint presentations
weight: 2
description: "Learn how to extract metadata from PowerPoint presentations (.ppt, .pptx) using GroupDocs.Parser for Java. Extract document properties like author, title, creation date, and comments from presentation files."
keywords: "extract PowerPoint metadata, .ppt metadata extraction, .pptx metadata parser, PowerPoint document properties, presentation metadata API, Java PowerPoint parser"
productName: GroupDocs.Parser for Java
hideChildren: False
---

## What is PowerPoint metadata?

PowerPoint metadata is like a "digital label" attached to your presentation files. It contains information about the file such as:

- Who created it
- When it was made
- The title and subject
- How long someone spent editing it
- Which version of PowerPoint was used

Think of it like the "Properties" you see when you right-click on a file - but now you can extract this information programmatically using Java code.

## Why extract PowerPoint metadata?

Here are some common reasons developers extract metadata from PowerPoint files:

- **File organization**: Automatically sort presentations by author, date, or company
- **Document tracking**: Find out who last modified a presentation and when
- **Content management**: Build searchable libraries of presentations
- **Compliance**: Track document history for auditing purposes

## Available metadata information

The [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method can extract these details from PowerPoint presentations:

| Name | What it tells you |
| --- | --- |
| **title** | The presentation title |
| **subject** | What the presentation is about |
| **keywords** | Tags or keywords for the presentation |
| **comments** | Any comments added to the presentation |
| **content-status** | The content status of the presentation |
| **category** | The category of the presentation |
| **company** | The company name associated with the file |
| **manager** | The manager associated with the presentation |
| **author** | Who created the presentation |
| **last-author** | Who last saved the presentation |
| **hyperlink-base** | The base string used for evaluating relative hyperlinks in the presentation |
| **application** | Which application created the presentation |
| **application-version** | The version of the application that created it |
| **created-time** | When the presentation was first created |
| **last-saved-time** | When someone last saved changes |
| **last-printed-time** | When the presentation was last printed |
| **revision-number** | How many times the file has been revised |
| **total-editing-time** | How many minutes were spent editing |

Properties that are empty in the presentation are not included in the result.

## How to extract the metadata

It's really simple! Just follow these 3 steps:

### Step 1: Create a Parser

First, instantiate the [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object for your PowerPoint file:

```java
try (Parser parser = new Parser("your-presentation.pptx")) {
    // Your code goes here
}
```

### Step 2: Get the metadata

Call the [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method to obtain the collection of [MetadataItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/MetadataItem) objects:

```java
Iterable<MetadataItem> metadata = parser.getMetadata();
```

### Step 3: Read the information

Loop through the results and print out what you found:

```java
for (MetadataItem item : metadata) {
    System.out.println(item.getName() + ": " + item.getValue());
}
```

### Complete example

Here's the full working example:

```java
// Create an instance of Parser class
try (Parser parser = new Parser(Constants.SamplePptx)) {
    // Extract metadata from the presentation
    Iterable<MetadataItem> metadata = parser.getMetadata();
    // Iterate over metadata items
    for (MetadataItem item : metadata) {
        // Print an item name and value
        System.out.println(String.format("%s: %s", item.getName(), item.getValue()));
    }
}
```

## What file types work?

This works with all common PowerPoint formats:

- **.ppt** - Older PowerPoint files
- **.pptx** - Newer PowerPoint files
- **.pps** - PowerPoint slideshow files
- **.ppsx** - Newer slideshow files

## Important notes

{{< alert style="warning" >}}[getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method returns null value if metadata extraction isn't supported for the document. For example, metadata extraction isn't supported for TXT files. Therefore, for TXT file [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method returns null. If Microsoft Office PowerPoint presentation has no metadata, [getMetadata](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getMetadata()) method returns an empty collection.{{< /alert >}}

## Real-world example

Let's say you have a folder full of PowerPoint presentations and you want to find out who created each one:

```java
try (DirectoryStream<Path> presentations = Files.newDirectoryStream(Paths.get("C:\\MyPresentations"), "*.pptx")) {
    for (Path presentation : presentations) {
        try (Parser parser = new Parser(presentation.toString())) {
            String author = "Unknown";
            Iterable<MetadataItem> metadata = parser.getMetadata();
            if (metadata != null) {
                for (MetadataItem item : metadata) {
                    if ("author".equals(item.getName())) {
                        author = item.getValue();
                    }
                }
            }

            System.out.println("File: " + presentation.getFileName());
            System.out.println("Author: " + author);
            System.out.println("---");
        }
    }
}
```

This will show you the author of each PowerPoint file in your folder!

## Common use cases

**For businesses:**
- Track which employee created which presentation
- Find presentations by topic or keyword
- Monitor when presentations were last updated

**For developers:**
- Build document management systems
- Create automated file organizing tools
- Add search functionality to presentation libraries

**For content managers:**
- Organize presentation libraries
- Track document versions and changes
- Ensure proper attribution and ownership

## Troubleshooting

**Problem**: Getting no metadata back?
- **Solution**: The file might not have metadata, or the file format doesn't support metadata extraction (in this case `getMetadata` returns null)

**Problem**: Can't read the file?
- **Solution**: Make sure the file path is correct. For a password-protected presentation, pass the password with [LoadOptions](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/LoadOptions) (see [Password-protected documents]({{< ref "parser/java/developer-guide/advanced-usage/loading/password-protected-documents.md" >}}))

**Problem**: Getting an error?
- **Solution**: Check that the file isn't corrupted and is a valid PowerPoint file

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)   
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
