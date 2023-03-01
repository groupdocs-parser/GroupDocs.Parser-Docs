---
id: handle-loading-of-external-resources
url: parser/java/handle-loading-of-external-resources
title: Handle loading of external resources documents
weight: 5
description: ""
keywords: 
productName: GroupDocs.Parser for Java
hideChildren: False
---
GroupDocs.Parser provides the functionality to handle loading of HTML external resources.

Here are the steps to handle loading of HTML external resources.

*   Instantiate the [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings/) object and pass External Resource Handler;
*   Create [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/) object and call [GetImages](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/#getImages--) method.

The following code sample shows how to handle loading of HTML external resources.

```java
// Create an instance of ParserSettings to pass External Resource Handler
ParserSettings settings = new ParserSettings(new Handler());
// Create an instance of Parser class to generate spreadsheet page previews
try (Parser parser = new Parser(Constants.SampleHtmlWithImages, settings)) {
    // Extract images from HTML document
    Iterable<PageImageArea> images = parser.getImages();
    // Iterate over extracted images
    for (PageImageArea i : images) {
        // Print the type of image
        System.out.println(i.getFileType());
    }
}

/**
 * This class provides the ability to filter extracted images.
 **/
class Handler extends ExternalResourceHandler {
    // Called before any external resource loads. It allows to skip unnecessary file loading.
    @Override
    public void onLoading(ExternalResourceLoadingArgs args) {
        // Check if the file name ends with installation.png
        if (!args.getUri().endsWith("installation.png")) {
            // Otherwise skip this file
            args.setSkipped(true);
        }

        super.onLoading(args);
    }
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).