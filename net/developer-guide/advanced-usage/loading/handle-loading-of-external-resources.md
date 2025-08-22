---
id: handle-loading-of-external-resources
url: parser/net/handle-loading-of-external-resources
title: Handle loading of external resources documents
weight: 4
description: "Learn how to handle loading of external resources."
keywords: open HTML handle loading of external resources documents
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---
GroupDocs.Parser provides the functionality to handle loading of HTML external resources.

Here are the steps to handle loading of HTML external resources.

*   Instantiate the [ParserSettings](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/parsersettings) object and pass External Resource Handler;
*   Create [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object and call [GetImages](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/getimages/) method.

The following code sample shows how to handle loading of HTML external resources.

```csharp
// Create an instance of ParserSettings to pass External Resource Handler
ParserSettings settings = new ParserSettings(new Handler());

// Create an instance of Parser class to generate spreadsheet page previews
using (Parser parser = new Parser(Constants.SampleHtmlWithImages, settings))
{
    // Extract images from HTML document
    IEnumerable<PageImageArea> images = parser.GetImages();

    // Iterate over extracted images
    foreach (PageImageArea i in images)
    {
        // Print the type of image
        Console.WriteLine(i.FileType);
    }
}

/// <summary>
/// This class provides the ability to filter extracted images.
/// </summary>
private class Handler : ExternalResourceHandler
{
    /// <summary>
    /// Called before any external resource loads. It allows to skip unnesesary file loading.
    /// </summary>
    public override void OnLoading(ExternalResourceLoadingArgs args)
    {
        // Check if the file name ends with installation.png
        if (!args.Uri.EndsWith("installation.png"))
        {
            // Otherwise skip this file
            args.Skipped = true;
        }

        base.OnLoading(args);
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