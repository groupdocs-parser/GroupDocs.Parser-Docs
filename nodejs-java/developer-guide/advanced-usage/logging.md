---
id: logging
url: parser/nodejs-java/logging
title: Logging
weight: 103
description: "Learn how to receive errors, warnings and events of data extraction with an ILogger implemented in JavaScript in GroupDocs.Parser for Node.js via Java."
keywords: logging, ILogger, data extraction errors, java.newProxy, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

The `ILogger` interface is used to receive the information about errors, warnings and events which occur while data extraction. The `ILogger` interface has the following members:

| Member | Description |
| --- | --- |
| `error(String, java.lang.Exception)` | Logs an error that occurred during data extraction. |
| `warning(String)` | Logs a warning that occurred during data extraction. |
| `trace(String)` | Logs an event occurred during data extraction. |

`ILogger` is a Java interface, so it can be implemented in JavaScript with `java.newProxy` of the `java` module. The methods of the proxy are called synchronously while the parser works.

Here are the steps to receive the information via the `ILogger` interface:

* Implement the `ILogger` interface with `java.newProxy('com.groupdocs.parser.options.ILogger', {...})`;
* Create an instance of the `ParserSettings` class with the logger;
* Create an instance of the `Parser` class with the `ParserSettings` object.

The following example shows how to receive the information via the `ILogger` interface. The password of the document isn't set, so the logger receives the error:

```javascript
const java = require('java');
const groupdocs = require('@groupdocs/groupdocs.parser');

// Implement ILogger interface in JavaScript
const logger = java.newProxy('com.groupdocs.parser.options.ILogger', {
  error: (message, exception) => {
    // Print error message
    console.log('Error: ' + message + (exception ? ' (' + exception.getClass().getName() + ')' : ''));
  },
  trace: (message) => {
    // Print event message
    console.log('Event: ' + message);
  },
  warning: (message) => {
    // Print warning message
    console.log('Warning: ' + message);
  },
});

try {
  // Create an instance of Parser class with the parser settings
  const parser = new groupdocs.Parser('samplePassword.pdf', null, new groupdocs.ParserSettings(logger));
  try {
    // Check if text extraction is supported
    if (!parser.getFeatures().isText()) {
      console.log("Text extraction isn't supported.");
    } else {
      // Print the document text
      const reader = parser.getText();
      try {
        console.log(reader.readToEnd());
      } finally {
        reader.close();
      }
    }
  } finally {
    parser.close();
  }
} catch (err) {
  // Ignore the exception; it's already logged
}
process.exit(0);
```

The output:

```
Error: null (com.groupdocs.parser.exceptions.InvalidPasswordException)
```

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
