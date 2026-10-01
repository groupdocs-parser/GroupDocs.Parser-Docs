---
id: extract-images-from-document
url: parser/nodejs-java/extract-images-from-document
title: Extract images from document
weight: 1
description: "Learn how to extract all images from a document in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: extract images from document, extract images, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser provides the functionality to extract images from documents by the `getImages()` method:

```javascript
parser.getImages() // returns Iterable<PageImageArea> or null
```

This method returns a Java `Iterable` collection of `PageImageArea` objects:

| Member | Description |
| --- | --- |
| `getPage()` | The page that contains the image. |
| `getRectangle()` | The rectangular area on the page that contains the image. |
| `getFileType()` | The format of the image. |
| `getRotation()` | The rotation angle of the image. |
| `getImageStream()` | Returns the image stream (`java.io.InputStream`). |
| `getImageStream(ImageOptions)` | Returns the image stream in a different format. |
| `save(String)` | Saves the image to the file. |
| `save(String, ImageOptions)` | Saves the image to the file in a different format. |

The `ImageOptions` class is used to define the image format into which the image is converted. The following image formats (`ImageFormat`) are supported:

* `Bmp`
* `Gif`
* `Jpeg`
* `Png`
* `WebP`

Here are the steps to extract images from the whole document:

* Instantiate the `Parser` object for the initial document;
* Call the `getImages()` method and obtain the collection of `PageImageArea` objects;
* Check if the collection isn't `null` (images extraction is supported for the document);
* Iterate through the collection and get sizes, image types and image contents.

The following example shows how to extract all images from the whole document:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('images.pdf');
try {
  // Extract images
  const images = parser.getImages();
  // Check if images extraction is supported
  if (images == null) {
    console.log("Images extraction isn't supported");
  } else {
    // Iterate over images
    const it = images.iterator();
    while (it.hasNext()) {
      const image = it.next();
      // Print a page index, rectangle and image type
      console.log(`Page: ${image.getPage().getIndex()}, R: ${image.getRectangle()}, Type: ${image.getFileType()}`);
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

The output for `images.pdf` starts with:

```text
Page: 0, R: (342.3999938964844; 233.83001708984375) (247.9499969482422; 164.8800048828125), Type: JPEG Image (.jpeg)
Page: 0, R: (342.3599853515625; 244.08001708984375) (248.0399932861328; 6.71999979019165), Type: JPEG Image (.jpeg)
...
```

To save the extracted images to files, see [Extract images to files](/parser/nodejs-java/extract-images-to-files/).

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
