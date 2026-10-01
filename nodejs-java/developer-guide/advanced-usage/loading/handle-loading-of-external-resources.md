---
id: handle-loading-of-external-resources
url: parser/nodejs-java/handle-loading-of-external-resources
title: Handle loading of external resources documents
weight: 5
description: "Learn how to filter or skip external resources of HTML documents with ExternalResourceHandler in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: external resources, ExternalResourceHandler, HTML images, skip loading, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser provides the functionality to handle loading of external resources of HTML documents (for example, images referenced by `<img>` tags).

The handler is set by the `ParserSettings` object. It must be an instance of the `ExternalResourceHandler` class (or its subclass) with the overridden `onLoading(ExternalResourceLoadingArgs)` method. `ExternalResourceLoadingArgs` has the following members:

| Member | Description |
| --- | --- |
| `getUri()` | The URI of the external resource. |
| `setUri(String)` | Changes the URI of the external resource. |
| `isSkipped()`, `setSkipped(boolean)` | Gets or sets the value that indicates whether the resource is skipped. |
| `getData()`, `setData(byte[])` | Gets or sets the content of the resource. |

## Create the handler class

`ExternalResourceHandler` is a Java class, not an interface. The `java` module can implement only Java interfaces in JavaScript (`java.newProxy`), so the subclass must be written in Java. The following small class forwards `onLoading` calls to `java.util.function.Consumer`, which can be implemented in JavaScript:

```java
package com.example;

import com.groupdocs.parser.options.ExternalResourceHandler;
import com.groupdocs.parser.options.ExternalResourceLoadingArgs;
import java.util.function.Consumer;

// Forwards onLoading calls to a callback that can be implemented in JavaScript
public class CallbackResourceHandler extends ExternalResourceHandler {
    private final Consumer<ExternalResourceLoadingArgs> callback;

    public CallbackResourceHandler(Consumer<ExternalResourceLoadingArgs> callback) {
        this.callback = callback;
    }

    @Override
    public void onLoading(ExternalResourceLoadingArgs args) {
        callback.accept(args);
        super.onLoading(args);
    }
}
```

Save it as `com/example/CallbackResourceHandler.java` and compile it into a JAR with the JDK against the JAR of the package:

```bash
javac --release 8 -cp node_modules/@groupdocs/groupdocs.parser/lib/groupdocs-parser-nodejs-26.9.jar -d out com/example/CallbackResourceHandler.java
jar cf handlers.jar -C out .
```

## Use the handler

Here are the steps to handle loading of external resources:

* Add the JAR with the handler class to `java.classpath` before the first call to the package;
* Implement the callback with `java.newProxy('java.util.function.Consumer', ...)`;
* Create an instance of the `ParserSettings` class with the handler;
* Create an instance of the `Parser` class with the settings and call the `getImages` method.

The following code sample shows how to load only the `installation.png` image of the HTML document and skip other images:

```javascript
const path = require('path');
const java = require('java');
// Add the JAR with the handler class before the JVM starts
java.classpath.push(path.join(__dirname, 'handlers.jar'));
const groupdocs = require('@groupdocs/groupdocs.parser');

const CallbackResourceHandler = java.import('com.example.CallbackResourceHandler');

// Called before any external resource loads. It allows to skip unnecessary file loading.
const onLoading = java.newProxy('java.util.function.Consumer', {
  accept: (args) => {
    // Check if the file name ends with installation.png
    if (!args.getUri().endsWith('installation.png')) {
      // Otherwise skip this file
      args.setSkipped(true);
    }
  },
});

// Create an instance of ParserSettings to pass External Resource Handler
const settings = new groupdocs.ParserSettings(new CallbackResourceHandler(onLoading));
// Create an instance of Parser class with the settings
const parser = new groupdocs.Parser('installation.html', settings);
try {
  // Extract images from HTML document
  const images = parser.getImages();
  // Iterate over extracted images
  const it = images.iterator();
  while (it.hasNext()) {
    // Print the type of image
    console.log(String(it.next().getFileType()));
  }
} finally {
  parser.close();
}
process.exit(0);
```

`installation.html` references `installation.png`, `installation_1.png` and `installation_2.png`. Without the handler three images are extracted; with the handler only one:

```
Portable Network Graphic (.png)
```

{{< alert style="info" >}}
`java.classpath.push` has effect only before the JVM is created. Call it before the first call to `@groupdocs/groupdocs.parser` (for example, before setting the license).
{{< /alert >}}

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
