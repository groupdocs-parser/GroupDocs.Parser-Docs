---
id: password-protected-documents
url: parser/nodejs-java/password-protected-documents
title: Password-protected documents
weight: 4
description: "Learn how to open password-protected documents and check whether a file is encrypted in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: password-protected documents, InvalidPasswordException, encrypted, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser provides the functionality to open password-protected documents.

The following are the steps to work with password-protected documents:

* Create an instance of the `LoadOptions` class;
* Set the password in the `LoadOptions(String)` constructor;
* Create an instance of the `Parser` class with these options and call any method.

The following code sample shows how to process password-protected documents:

```javascript
const java = require('java');
const groupdocs = require('@groupdocs/groupdocs.parser');

try {
  const password = '123456';
  // Create an instance of Parser class with the password
  const parser = new groupdocs.Parser('samplePassword.pdf', new groupdocs.LoadOptions(password));
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
  // The Java exception is available in err.cause
  if (err.cause && java.instanceOf(err.cause, 'com.groupdocs.parser.exceptions.InvalidPasswordException')) {
    // Print the message if the password is incorrect or empty
    console.log('Invalid password');
  } else {
    throw err;
  }
}
process.exit(0);
```

If the password is incorrect or empty, `InvalidPasswordException` is thrown. The error thrown in JavaScript wraps the Java exception: check it with `java.instanceOf(err.cause, ...)`.

The following code shows how to check whether a file is password-protected:

```javascript
const groupdocs = require('@groupdocs/groupdocs.parser');

// Get a file info
const info = groupdocs.Parser.getFileInfo('samplePassword.pdf');
// Check isEncrypted property
console.log(info.isEncrypted() ? 'Password is required' : 'Password is not required');
process.exit(0);
```

## More resources

### Free online document parser App

Along with the full-featured library we provide simple but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
