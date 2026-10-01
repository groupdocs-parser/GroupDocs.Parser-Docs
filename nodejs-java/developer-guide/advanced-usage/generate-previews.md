---
id: generate-previews
url: parser/nodejs-java/generate-previews
title: Generate previews
weight: 105
description: "Learn how to generate document page previews as PNG images in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: page previews, generate page previews, document preview, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser provides the functionality to generate document page previews by the `generatePreview(PreviewOptions)` method:

```javascript
parser.generatePreview(previewOptions)
```

The `PreviewOptions` class is used to set requirements and stream callbacks for preview generation. The callbacks are Java interfaces, so in Node.js you implement them with `java.newProxy` from the `java` package:

| Interface | Method | Description |
| --- | --- | --- |
| `com.groupdocs.parser.options.ICreatePageStream` | `createPageStream(pageNumber)` | Returns a `java.io.OutputStream` for the page preview. Required. |
| `com.groupdocs.parser.options.IReleasePageStream` | `releasePageStream(pageNumber, stream)` | Called when the page preview is written. Optional. |
| `com.groupdocs.parser.options.IPreviewPageRender` | `previewPageRender(pageRenderInfo)` | Receives `PageRenderInfo` before a page is rendered. Optional. |

`PreviewOptions` also has `setPreviewFormat` (`PreviewFormats.Png`, `PreviewFormats.Jpeg`, `PreviewFormats.Bmp`), `setDpi`, `setWidth`, `setHeight` and `setPageNumbers`.

Here are the steps to generate document page previews:

* Prepare the `PreviewOptions` object;
* Instantiate the `Parser` object;
* Call the `generatePreview(PreviewOptions)` method.

The following example shows how to generate document page previews:

```javascript
const fs = require('fs');
const path = require('path');
const java = require('java');
const groupdocs = require('@groupdocs/groupdocs.parser');

const FileOutputStream = java.import('java.io.FileOutputStream');
const outputDir = 'previews';
fs.mkdirSync(outputDir, { recursive: true });

// Create an instance of Parser class to generate document page previews
const parser = new groupdocs.Parser('SampleWithToc.pdf');
try {
  // Implement ICreatePageStream: return a Java output stream for each page
  const createPageStream = java.newProxy('com.groupdocs.parser.options.ICreatePageStream', {
    createPageStream: (pageNumber) =>
      new FileOutputStream(path.join(outputDir, `preview_${pageNumber}.png`)),
  });
  // Create preview options
  const previewOptions = new groupdocs.PreviewOptions(createPageStream);
  // Set PNG as an output image format
  previewOptions.setPreviewFormat(groupdocs.PreviewFormats.Png);
  // Set DPI for the output image
  previewOptions.setDpi(72);
  // Generate previews
  parser.generatePreview(previewOptions);
} finally {
  parser.close();
}
process.exit(0);
```

For the three-page `SampleWithToc.pdf` this creates `preview_1.png`, `preview_2.png` and `preview_3.png` (612 x 792 pixels at 72 DPI).

## Close page streams

To control when the page stream is closed, pass an `IReleasePageStream` implementation as the second constructor argument:

```javascript
// Implement IReleasePageStream: close the stream when the page preview is written
const releasePageStream = java.newProxy('com.groupdocs.parser.options.IReleasePageStream', {
  releasePageStream: (pageNumber, stream) => stream.close(),
});
// Create preview options with both callbacks
const previewOptions = new groupdocs.PreviewOptions(createPageStream, releasePageStream);
```

## Generate spreadsheet previews

{{< alert style="info" >}}Spreadsheets are rendered by tiles. See the example below.{{< /alert >}}

Here are the steps to generate spreadsheet page previews:

* Prepare the `PreviewOptions` object;
* Set the `IPreviewPageRender` callback to obtain the `PageRenderInfo` (row and column of each tile);
* Instantiate the `Parser` object;
* Call the `generatePreview(PreviewOptions)` method.

The following example shows how to generate spreadsheet page previews:

```javascript
const fs = require('fs');
const path = require('path');
const java = require('java');
const groupdocs = require('@groupdocs/groupdocs.parser');

const FileOutputStream = java.import('java.io.FileOutputStream');
const outputDir = 'previews';

// Get the output file path. If the render info is set, then tile name is {PageNumber}/{Row}x{Column}.png
function getOutputPath(renderInfo, pageNumber) {
  const fileName = renderInfo == null
    ? `preview_${pageNumber}.png`
    : path.join(`${renderInfo.getPageNumber()}`,
      `${renderInfo.getRow(pageNumber)}x${renderInfo.getColumn(pageNumber)}.png`);
  const filePath = path.join(outputDir, fileName);
  fs.mkdirSync(path.dirname(filePath), { recursive: true });
  return filePath;
}

// Create an instance of Parser class to generate spreadsheet page previews
const parser = new groupdocs.Parser('sample.xlsx');
try {
  let renderInfo = null;
  // Create preview options
  const previewOptions = new groupdocs.PreviewOptions(
    java.newProxy('com.groupdocs.parser.options.ICreatePageStream', {
      createPageStream: (pageNumber) => new FileOutputStream(getOutputPath(renderInfo, pageNumber)),
    }));
  // Set the callback to obtain the render info
  previewOptions.setPreviewPageRender(
    java.newProxy('com.groupdocs.parser.options.IPreviewPageRender', {
      previewPageRender: (pageRenderInfo) => { renderInfo = pageRenderInfo; },
    }));
  // Set PNG as an output image format
  previewOptions.setPreviewFormat(groupdocs.PreviewFormats.Png);
  // Set DPI for the output image
  previewOptions.setDpi(72);
  // Generate previews
  parser.generatePreview(previewOptions);
} finally {
  parser.close();
}
process.exit(0);
```

For `sample.xlsx` (two worksheets) this creates `previews/1/0x0.png` and `previews/2/0x0.png`.

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
