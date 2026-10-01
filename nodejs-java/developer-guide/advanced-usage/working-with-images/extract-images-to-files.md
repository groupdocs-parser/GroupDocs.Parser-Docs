---
id: extract-images-to-files
url: parser/nodejs-java/extract-images-to-files
title: Extract images to files
weight: 4
description: "Learn how to save images extracted from documents to files or Node.js Buffers with GroupDocs.Parser for Node.js via Java."
keywords: extract images to files, extract images, save images, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

Here are the steps to extract images to files:

* Instantiate the `Parser` object for the initial document;
* Call the `getImages()` method and obtain the collection of `PageImageArea` objects;
* Check if the collection isn't `null` (images extraction is supported for the document);
* Iterate through the collection and save image contents to the file.

## Save images with PageImageArea.save

The following example shows how to save images to PNG files:

```javascript
const fs = require('fs');
const path = require('path');
const groupdocs = require('@groupdocs/groupdocs.parser');

const outputDir = 'output';
fs.mkdirSync(outputDir, { recursive: true });

// Create an instance of Parser class
const parser = new groupdocs.Parser('sample.zip');
try {
  // Extract images from document
  const images = parser.getImages();
  // Check if images extraction is supported
  if (images == null) {
    console.log("Page images extraction isn't supported");
  } else {
    // Create the options to save images in PNG format
    const options = new groupdocs.ImageOptions(groupdocs.ImageFormat.Png);
    let imageNumber = 0;
    // Iterate over images
    const it = images.iterator();
    while (it.hasNext()) {
      const image = it.next();
      // Save the image to the png file
      image.save(path.join(outputDir, `${imageNumber}.png`), options);
      imageNumber++;
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

## Get images as Node.js Buffers

`getImageStream(ImageOptions)` returns a Java `InputStream`. Read it with `readAllBytes()` and wrap the result in a Node.js `Buffer` when you need to process the image data in JavaScript (for example, to upload it or pass it to another library):

```javascript
const fs = require('fs');
const path = require('path');
const groupdocs = require('@groupdocs/groupdocs.parser');

const outputDir = 'output';
fs.mkdirSync(outputDir, { recursive: true });

// Create an instance of Parser class
const parser = new groupdocs.Parser('images.pdf');
try {
  // Extract images from document
  const images = parser.getImages();
  // Check if images extraction is supported
  if (images == null) {
    console.log("Images extraction isn't supported");
  } else {
    // Create the options to convert images to PNG format
    const options = new groupdocs.ImageOptions(groupdocs.ImageFormat.Png);
    let imageNumber = 0;
    // Iterate over images
    const it = images.iterator();
    while (it.hasNext()) {
      const image = it.next();
      // Get the image as a Java InputStream
      const stream = image.getImageStream(options);
      try {
        // Read all bytes and convert them to a Node.js Buffer
        const buffer = Buffer.from(stream.readAllBytes());
        fs.writeFileSync(path.join(outputDir, `${imageNumber}.png`), buffer);
      } finally {
        stream.close();
      }
      imageNumber++;
    }
  }
} finally {
  parser.close();
}
process.exit(0);
```

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
