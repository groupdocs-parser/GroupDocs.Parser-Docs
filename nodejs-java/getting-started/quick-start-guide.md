---
id: quick-start-guide
url: parser/nodejs-java/quick-start-guide
title: Quick Start Guide
weight: 5
description: "Extract text from a document with GroupDocs.Parser for Node.js via Java in a few lines of JavaScript."
keywords: quick start, extract text, node.js, javascript, pdf
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

This guide shows how to extract text from a PDF document with GroupDocs.Parser for Node.js via Java.

Before you start, [install](/parser/nodejs-java/installation/) the `@groupdocs/groupdocs.parser` package.

## Extract text from a document

Create `app.js` next to the document you want to parse, for example [sample.pdf](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java/blob/master/Examples/Resources/SampleFiles/sample.pdf?raw=true):

```javascript
'use strict';

const groupdocs = require('@groupdocs/groupdocs.parser');

// Create an instance of the Parser class for the document
const parser = new groupdocs.Parser("sample.pdf");
try {
    // Extract the text into a reader
    const reader = parser.getText();

    // Print the text, or a message if text extraction isn't supported for this format
    console.log(reader === null ? "Text extraction isn't supported" : reader.readToEnd());
    if (reader !== null) {
        reader.close();
    }
} finally {
    // Release the document
    parser.close();
}

process.exit(0);
```

Run the script:

```bash
node app.js
```

The script prints the text of the document:

```
Lorem
Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Maecenas porttitor congue massa. Fusce posuere,
magna sed pulvinar ultricies, purus lectus malesuada libero, sit amet commodo magna eros quis urna.
...
```

{{< alert style="info" >}}
GroupDocs.Parser runs inside a Java virtual machine started by the `java` bridge, and the JVM keeps the Node.js process alive after your code finishes. Call `process.exit()` at the end of standalone scripts. Without a license, the extracted text is limited and contains evaluation marks; see [Licensing and evaluation](/parser/nodejs-java/evaluation-limitations-and-licensing/).
{{< /alert >}}

## How it works

- `Parser` opens the document. Always call `close()` when you are done to release the file.
- `getText()` returns a reader with the document text, or `null` if text extraction isn't supported for the document format.
- Methods of GroupDocs.Parser for Node.js via Java have the same names as in GroupDocs.Parser for Java and are called synchronously.

## Next steps

- [Extract text from documents](/parser/nodejs-java/extract-text-from-documents/)
- [Extract images from documents](/parser/nodejs-java/extract-images-from-documents/)
- [Extract metadata from documents](/parser/nodejs-java/extract-metadata-from-documents/)
- [Parse data from documents](/parser/nodejs-java/parse-data-from-documents/)
