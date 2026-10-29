---
id: handle-loading-of-external-resources
url: parser/java/handle-loading-of-external-resources
title: Handle loading of external resources documents
weight: 5
description: ""
keywords: 
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---
GroupDocs.Parser provides the functionality to handle loading of HTML external resources.

Here are the steps to handle loading of HTML external resources.

*   Instantiate the [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings/) object and pass External Resource Handler;
*   Create [Parser](https://reference.groupdocs.com/parser/java/com.groupdocs.parser/parser/) object and call [getImages](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getImages()) method.

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

[ExternalResourceLoadingArgs](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/ExternalResourceLoadingArgs) class provides the following members to control the loading of a resource:

| Member | Description |
| --- | --- |
| [getUri](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/ExternalResourceLoadingArgs#getUri()) / [setUri](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/ExternalResourceLoadingArgs#setUri(java.lang.String)) | Gets or sets the URI of the external resource. Set a new value to load the resource from another location. |
| [isSkipped](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/ExternalResourceLoadingArgs#isSkipped()) / [setSkipped](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/ExternalResourceLoadingArgs#setSkipped(boolean)) | Gets or sets the value that indicates whether the loading of the external resource must be skipped. |
| [getData](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/ExternalResourceLoadingArgs#getData()) / [setData](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/ExternalResourceLoadingArgs#setData(byte[])) | Gets or sets the content of the external resource. Set the bytes to provide the resource content yourself instead of loading it. |

{{< alert style="warning" >}}
Loading of external resources is enabled by default, which means that parsing a document from an untrusted source may cause outbound HTTP requests to hosts named inside that document — for example remote images or stylesheets referenced by an HTML or MHTML file or by the HTML body of an email.

`ExternalResourceHandler` is applied to HTML, MHTML and XHTML documents only, when the document is laid out: image extraction, page areas and the page count. Other extraction methods (for example, `getFormattedText`) and other formats, including email messages, do not route through it. To guarantee that no outbound request is made, block egress at the network or process level. See [Network access and data privacy]({{< ref "parser/java/getting-started/network-access-and-data-privacy.md" >}}) for details.
{{< /alert >}}

## External resources in email messages

The HTML body of an email message can refer to external resources, for example images stored on a remote server. Whether they are downloaded depends on the method that is called:

| Method | External images are downloaded |
| --- | --- |
| [getText](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getText()) | No: a plain text doesn't require the external images |
| [getFormattedText](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getFormattedText(com.groupdocs.parser.options.FormattedTextOptions)) | Yes: the message body is laid out as an HTML document |
| [getStructure](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getStructure()) | Yes |

Email messages don't support `ExternalResourceHandler` yet: `onLoading` isn't called and the external images are still downloaded. Hyperlinks from a message body are never followed: the parser downloads external resources like images and style sheets only.

The following example shows the calls that cause the external images of an email message to be downloaded:

```java
// Extract a plain text; the external image isn't required to build it
try (Parser parser = new Parser(filePath)) {
    try (TextReader reader = parser.getText()) {
        reader.readToEnd();
    }
}

// Extract a formatted text; the message body is laid out as an HTML document,
// therefore the external image is downloaded
try (Parser parser = new Parser(filePath)) {
    try (TextReader reader = parser.getFormattedText(new FormattedTextOptions(FormattedTextMode.Html))) {
        reader.readToEnd();
    }
}

// Extract a structure; the external image is downloaded as well
try (Parser parser = new Parser(filePath)) {
    parser.getStructure();
}
```

The complete example, which also prints the requested addresses, is available as `ExternalResourcesInEmailMessages` in the GitHub examples.

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).