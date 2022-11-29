---
id: groupdocs-parser-for-net-22-8-release-notes
url: parser/net/groupdocs-parser-for-net-22-8-release-notes
title: GroupDocs.Parser for .NET 22.8 Release Notes
weight: 2
description: ""
keywords: 
productName: GroupDocs.Parser for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Parser for .NET 22.8{{< /alert >}}

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| PARSERNET-1906 | Implement the support for 7z archives | New Feature |
| PARSERNET-1903 | Implement the support for attachment extraction from presentations | New Feature |
| PARSERNET-1904 | Implement the support for attachment extraction from spreadsheets | New Feature |
| PARSERNET-1905 | Implement the support for attachment extraction from word processing documents | New Feature |
| PARSERNET-1907 | ParserGetContainer details in API reference page is ambiguous | Bug |
| PARSERNET-1908 | New file format support | New Feature |

## Public API and Backward Incompatible Changes

### Implement the support for 7z archives

#### Description

This feature provides the ability to extract data from 7z archives.

#### Public API changes

[GroupDocs.Parser.Options.FileType](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/filetype) public class was updated with changes as follows:

* Added [SEVENZ](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/filetype/fields/sevenz) readonly field.

#### Usage

The following example shows how to extract a text from 7z entities:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser(filePath))
{
    // Extract attachments from the container
    IEnumerable<ContainerItem> attachments = parser.GetContainer();
    // Check if container extraction is supported
    if (attachments == null)
    {
        Console.WriteLine("Container extraction isn't supported");
    }
    // Iterate over 7z entities
    foreach (ContainerItem item in attachments)
    {
        // Print the file path
        Console.WriteLine(item.FilePath);
        try
        {
            // Create Parser object for the 7z entity content
            using (Parser attachmentParser = item.OpenParser())
            {
                // Extract an 7z entity text
                using (TextReader reader = attachmentParser.GetText())
                {
                    Console.WriteLine(reader == null ? "No text" : reader.ReadToEnd());
                }
            }
        }
        catch (UnsupportedDocumentFormatException)
        {
            Console.WriteLine("Isn't supported.");
        }
    }
}
```

### Implement the support for attachment extraction from presentations, spreadsheets and word processing documents

#### Description

These features provide the ability to extract attachments from documents.

#### Public API changes

No public API changes.

#### Usage

The following example shows how to extract a text from document attachments:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser(filePath))
{
    // Extract attachments from the container
    IEnumerable<ContainerItem> attachments = parser.GetContainer();
    // Check if container extraction is supported
    if (attachments == null)
    {
        Console.WriteLine("Container extraction isn't supported");
    }
    // Iterate over attachment entities
    foreach (ContainerItem item in attachments)
    {
        // Print the file path
        Console.WriteLine(item.FilePath);
        try
        {
            // Create Parser object for the attachment entity content
            using (Parser attachmentParser = item.OpenParser())
            {
                // Extract an attachment entity text
                using (TextReader reader = attachmentParser.GetText())
                {
                    Console.WriteLine(reader == null ? "No text" : reader.ReadToEnd());
                }
            }
        }
        catch (UnsupportedDocumentFormatException)
        {
            Console.WriteLine("Isn't supported.");
        }
    }
}
```

### New file format support

#### Description

This feature provides the ability to extract data from 7z archives and attachments from presentations, spreadsheets and word processing documents.

### ParserGetContainer details in API reference page is ambiguous

#### Description

Public API reference was updated: the link to the supported formats page was added.