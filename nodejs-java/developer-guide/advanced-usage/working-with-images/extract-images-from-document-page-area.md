---
id: extract-images-from-document-page-area
url: parser/nodejs-java/extract-images-from-document-page-area
title: Extract images from document page area
weight: 3
description: "Learn how to extract images from a rectangular page area in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: extract images from document page area, extract images, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser provides the functionality to extract images from a document page area by the `getImages(PageAreaOptions)` and `getImages(int, PageAreaOptions)` methods:

```javascript
parser.getImages(options)            // returns Iterable<PageImageArea> or null
parser.getImages(pageIndex, options) // returns Iterable<PageImageArea> or null
```

These methods return a Java `Iterable` collection of `PageImageArea` objects:

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

The `PageAreaOptions` parameter is used to customize the images extraction process. This class has the following members:

| Member | Description |
| --- | --- |
| `getRectangle()` | The rectangular area that contains images. |

Here are the steps to extract images from the upper-right area of the pages:

* Instantiate the `Parser` object for the initial document;
* Instantiate `PageAreaOptions` with the rectangular area;
* Call the `getImages(PageAreaOptions)` method and obtain the collection of `PageImageArea` objects;
* Check if the collection isn't `null` (images extraction is supported for the document);
* Iterate through the collection and get sizes, image types and image contents.

The following example shows how to extract only images from the specified page area:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('images.pdf');
try {
  // Create the options which are used for images extraction
  const options = new groupdocs.PageAreaOptions(
    new groupdocs.Rectangle(new groupdocs.Point(340, 150), new groupdocs.Size(300, 100)));
  // Extract images from the page area
  const images = parser.getImages(options);
  // Check if images extraction is supported
  if (images == null) {
    console.log("Page images extraction isn't supported");
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

The output for `images.pdf`:

```text
Page: 0, R: (342.3999938964844; 233.83001708984375) (247.9499969482422; 164.8800048828125), Type: JPEG Image (.jpeg)
Page: 0, R: (342.3599853515625; 244.08001708984375) (248.0399932861328; 6.71999979019165), Type: JPEG Image (.jpeg)
Page: 1, R: (392.54998779296875; 183.04998779296875) (191.10000610351562; 126.3499984741211), Type: JPEG Image (.jpeg)
```

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
