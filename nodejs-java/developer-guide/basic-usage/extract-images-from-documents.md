---
id: extract-images-from-documents
url: parser/nodejs-java/extract-images-from-documents
title: Extract images from documents
weight: 8
description: "Extract images from PDF, emails, ebooks, Microsoft Office and LibreOffice documents in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: extract images, extract images from pdf, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---
GroupDocs.Parser allows to extract images from PDF, Emails, Ebooks, Microsoft Office: Word (DOC, DOCX), PowerPoint (PPT, PPTX), Excel (XLS, XLSX), LibreOffice formats and many others (see the full list in the [supported document formats](/parser/nodejs-java/supported-document-formats/) article).

GroupDocs.Parser allows to easily implement both simple and complex image extraction cases (see more in the [working with images](/parser/nodejs-java/working-with-images/) section).

In this article you can see how to extract images from any supported format without additional settings.

## Extract images from documents

To extract images from documents simply call the `getImages()` method:

```javascript
parser.getImages(); // returns a Java Iterable of PageImageArea objects or null
```

This method returns a collection of `PageImageArea` objects:

| Member | Description |
| --- | --- |
| `getPage()` | The page that contains the image area. |
| `getRectangle()` | The rectangular area on the page that contains the image area. |
| `getFileType()` | The format of the image. |
| `getRotation()` | The rotation angle of the image. |
| `getImageStream()` | Returns the image stream. |
| `getImageStream(options)` | Returns the image stream in a different format. |
| `save(filePath)` | Saves the image to the file. |
| `save(filePath, options)` | Saves the image to the file in a different format. |

Here are the steps to extract images from the whole document:

*   Instantiate the `Parser` object for the initial document;
*   Call the `getImages()` method and obtain the collection of image objects;
*   Check if *collection* isn't *null* (image extraction is supported for the document);
*   Iterate through the collection and get sizes, image types and image contents.

The collection is a Java `Iterable`, so iterate over it with `iterator()`, `hasNext()` and `next()`.

The following example shows how to extract all images from the whole document:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('images.pdf');
try {
  // Extract images
  const images = parser.getImages();
  // Check if images extraction is supported
  if (images === null) {
    console.log("Images extraction isn't supported");
  } else {
    // Iterate over images
    const it = images.iterator();
    while (it.hasNext()) {
      const image = it.next();
      // Print a page index, rectangle and image type
      console.log(`Page: ${image.getPage().getIndex()}, R: ${image.getRectangle().toString()}, Type: ${image.getFileType().toString()}`);
    }
  }
} finally {
  parser.close();
}

process.exit(0);
```

## More resources

### Advanced usage topics

To learn more about document data extraction features and get familiar how to extract text, images, forms and more, please refer to the [advanced usage section](/parser/nodejs-java/advanced-usage/).

### Free online document parser App

Along with the full-featured library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
