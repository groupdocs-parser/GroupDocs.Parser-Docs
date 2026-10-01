---
id: extract-data-from-databases
url: parser/nodejs-java/extract-data-from-databases
title: Extract data from databases
weight: 100
description: "Learn how to extract tables from databases via JDBC in Node.js with GroupDocs.Parser for Node.js via Java."
keywords: via JDBC, extract data, database, SQLite, Node.js, JavaScript
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

GroupDocs.Parser provides the functionality to extract data from databases via JDBC. The list of tables is represented as the table of contents. The table extraction is processed by the `getText(pageIndex)` method: each table is a "page" of the document.

## Add the JDBC driver

The package doesn't contain JDBC drivers. Download the JAR of the driver for your database (for example, `org.xerial:sqlite-jdbc` for SQLite) and add it to `java.classpath` before the first call to the package:

```javascript
const path = require('path');
const java = require('java');
// Add the JDBC driver to the classpath before the JVM starts
java.classpath.push(path.join(__dirname, 'sqlite-jdbc-3.25.2.jar'));
const groupdocs = require('@groupdocs/groupdocs.parser');
```

`java.classpath.push` has effect only before the JVM is created, so call it before any other call to `@groupdocs/groupdocs.parser` (including setting the license).

## Extract data with Connection object

To create an instance of the `Parser` class to extract data from a database with a `java.sql.Connection` object the following constructors are used:

```javascript
new groupdocs.Parser(connection)
new groupdocs.Parser(connection, parserSettings)
```

The second constructor allows to use the `ParserSettings` object to control the process; for example, by adding [logging](/parser/nodejs-java/logging/) functionality.

Here are the steps to extract data from an SQLite database:

* Prepare the `java.sql.Connection` object with `java.sql.DriverManager`;
* Create an instance of the `Parser` class with the connection object;
* Call the `isText` method of `getFeatures()` to check if text extraction is supported;
* Call the `isToc` method of `getFeatures()` to check if table of contents extraction is supported;
* Call the `getToc` method and obtain the collection of tables;
* Iterate through the collection and get a text from tables.

The following example shows how to extract data from an SQLite database:

```javascript
const path = require('path');
const java = require('java');
// Add the JDBC driver to the classpath before the JVM starts
java.classpath.push(path.join(__dirname, 'sqlite-jdbc-3.25.2.jar'));
const groupdocs = require('@groupdocs/groupdocs.parser');

const DriverManager = java.import('java.sql.DriverManager');

// Create the java.sql.Connection object
const connection = DriverManager.getConnection('jdbc:sqlite:sqlite.db');
try {
  // Create an instance of Parser class to extract tables from the database
  const parser = new groupdocs.Parser(connection);
  try {
    if (!parser.getFeatures().isText()) {
      // Check if text extraction is supported
      console.log("Text extraction isn't supported.");
    } else if (!parser.getFeatures().isToc()) {
      // Check if toc extraction is supported
      console.log("Toc extraction isn't supported.");
    } else {
      // Get a list of tables
      const it = parser.getToc().iterator();
      // Iterate over tables
      while (it.hasNext()) {
        const item = it.next();
        // Print the table name
        console.log(item.getText());
        // Extract a table content as a text
        const reader = parser.getText(item.getPageIndex());
        try {
          console.log(reader.readToEnd());
        } finally {
          reader.close();
        }
      }
    }
  } finally {
    parser.close();
  }
} finally {
  connection.close();
}
process.exit(0);
```

The beginning of the output for `sqlite.db` (the table name is printed from the TOC item; the table text starts with the table name too):

```
a_table
a_table
id name
1 a1
2 a2
3 a3
...
```

## Extract data with connection string

To create an instance of the `Parser` class to extract data from a database with a connection string the following constructor is used:

```javascript
new groupdocs.Parser(connectionString, new groupdocs.LoadOptions(groupdocs.FileFormat.Database))
```

The JDBC driver must be on the classpath in this case too.

Here are the steps to extract data from an SQLite database:

* Prepare the JDBC connection string;
* Create an instance of the `Parser` class with the connection string and `LoadOptions` with `FileFormat.Database`;
* Call the `isText` method of `getFeatures()` to check if text extraction is supported;
* Call the `isToc` method of `getFeatures()` to check if table of contents extraction is supported;
* Call the `getToc` method and obtain the collection of tables;
* Iterate through the collection and get a text from tables.

The following example shows how to extract data from an SQLite database:

```javascript
const path = require('path');
const java = require('java');
// Add the JDBC driver to the classpath before the JVM starts
java.classpath.push(path.join(__dirname, 'sqlite-jdbc-3.25.2.jar'));
const groupdocs = require('@groupdocs/groupdocs.parser');

const connectionString = 'jdbc:sqlite:sqlite.db';
// Create an instance of Parser class to extract tables from the database
// As filePath connection parameters are passed; LoadOptions is set to Database file format
const parser = new groupdocs.Parser(connectionString, new groupdocs.LoadOptions(groupdocs.FileFormat.Database));
try {
  if (!parser.getFeatures().isText()) {
    // Check if text extraction is supported
    console.log("Text extraction isn't supported.");
  } else if (!parser.getFeatures().isToc()) {
    // Check if toc extraction is supported
    console.log("Toc extraction isn't supported.");
  } else {
    // Get a list of tables
    const it = parser.getToc().iterator();
    // Iterate over tables
    while (it.hasNext()) {
      const item = it.next();
      // Print the table name
      console.log(item.getText());
      // Extract a table content as a text
      const reader = parser.getText(item.getPageIndex());
      try {
        console.log(reader.readToEnd());
      } finally {
        reader.close();
      }
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

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our [Free Online Document Parser App](https://products.groupdocs.app/parser).
