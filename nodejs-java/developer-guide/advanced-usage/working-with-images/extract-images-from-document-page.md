---
id: extract-images-from-document-page
url: parser/nodejs-java/extract-images-from-document-page
title: Extract images from document page
weight: 2
description: "Learn how to extract images from a specific document page in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: extract images from document page, extract images, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser provides the functionality to extract images from a document page by the `getImages(int)` method:

```javascript
parser.getImages(pageIndex) // returns Iterable<PageImageArea> or null
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

Here are the steps to extract images from the document page:

* Instantiate the `Parser` object for the initial document;
* Call `parser.getFeatures().isImages()` to check if images extraction is supported for the document;
* Call the `getImages(int)` method with the page index and obtain the collection of `PageImageArea` objects;
* Iterate through the collection and get sizes, image types and image contents.

The following example shows how to extract images from each document page:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of Parser class
const parser = new groupdocs.Parser('images.pdf');
try {
  // Check if the document supports images extraction
  if (!parser.getFeatures().isImages()) {
    console.log("Document doesn't support images extraction.");
  } else {
    // Get the document info
    const documentInfo = parser.getDocumentInfo();
    const pageCount = documentInfo.getPageCount();
    // Iterate over pages
    for (let pageIndex = 0; pageIndex < pageCount; pageIndex++) {
      // Print a page number
      console.log(`Page ${pageIndex + 1}/${pageCount}`);
      // Iterate over images
      // We ignore null-checking as we have checked images extraction feature support earlier
      const it = parser.getImages(pageIndex).iterator();
      while (it.hasNext()) {
        const image = it.next();
        // Print a rectangle and image type
        console.log(`R: ${image.getRectangle()}, Type: ${image.getFileType()}`);
      }
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

The output for `images.pdf`:

```text
Page 1/2
R: (342.3999938964844; 233.83001708984375) (247.9499969482422; 164.8800048828125), Type: JPEG Image (.jpeg)
R: (342.3599853515625; 244.08001708984375) (248.0399932861328; 6.71999979019165), Type: JPEG Image (.jpeg)
R: (411.95001220703125; 626.4799957275391) (153.75; 102.41999816894531), Type: JPEG Image (.jpeg)
R: (411.9599914550781; 631.4400024414062) (153.72000122070312; 1.440000057220459), Type: JPEG Image (.jpeg)
Page 2/2
R: (85.05000305175781; 509.010009765625) (256.0199890136719; 204.75), Type: JPEG Image (.jpeg)
R: (392.54998779296875; 183.04998779296875) (191.10000610351562; 126.3499984741211), Type: JPEG Image (.jpeg)
```

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
