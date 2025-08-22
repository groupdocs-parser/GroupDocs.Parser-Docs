---
id: extract-attachments-from-emails
url: parser/net/extract-attachments-from-emails
title: Extract attachments from Emails
weight: 4
description: "Extract email attachments using GroupDocs.Parser for .NET"
keywords: Extract email attachments
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---
To extract attachments from emails [GetContainer](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getcontainer) method is used. This method returns the collection of [ContainerItem](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/containeritem) objects.

Email Attachment can contain the following metadata:

| Name | Description |
| --- | --- |
| content-type | The MIME type of the attachment content |

These metadata refer to a container element itself, not a document.

Here are the steps to extract an email text from email attachments:

*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
*   Call [GetContainer](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getcontainer) method and obtain collection of [ContainerItem](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/containeritem) objects;
*   Check if *collection* isn't *null* (container extraction is supported for the document);
*   Iterate through the collection and get container item names, sizes and obtain content.

The following example shows how to extract a text from email attachments:

```csharp
// Create an instance of Parser class
using(Parser parser = new Parser(filePath))
{
    // Extract attachments from the container
    IEnumerable<ContainerItem> attachments = parser.GetContainer();
    // Check if container extraction is supported
    if(attachments == null)
    {
        Console.WriteLine("Container extraction isn't supported");
    }

    // Iterate over attachments
    foreach(ContainerItem item in attachments)
    {
		// Print the file path
		Console.WriteLine(item.FilePath);
       
        // Print metadata
        foreach (MetadataItem metadata in item.Metadata)
        {
            Console.WriteLine(string.Format("{0}: {1}", metadata.Name, metadata.Value));
        }

		try
        {
            // Create Parser object for the email content
            using (Parser attachmentParser = item.OpenParser())
            {
                // Extract an email text
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

## Detect inline images

To detect if image attachment is inline 'disposition' metadata is used. If it's present, it can take 'inline' or 'attachment' values.

The following example shows how to detect inline attachments:

```csharp
// Create an instance of Parser class
using(Parser parser = new Parser(filePath))
{
    // Extract attachments from the container
    IEnumerable<ContainerItem> attachments = parser.GetContainer();
    // Check if container extraction is supported
    if(attachments == null)
    {
        Console.WriteLine("Container extraction isn't supported");
    }

    // Iterate over attachments
    foreach(ContainerItem item in attachments)
    {
        // Check metadata for 'disposition' item
        if(item.GetMetadataValue("disposition") == "inline") {
		    // If it's 'inline' then print the file path
		    Console.WriteLine(item.FilePath);
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