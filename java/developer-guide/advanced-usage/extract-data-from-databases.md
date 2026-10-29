---
id: extract-data-from-databases
url: parser/java/extract-data-from-databases
title: Extract data from databases
weight: 100
description: "Complete guide to extracting data from databases via JDBC using GroupDocs.Parser for Java. Learn how to connect to SQLite, SQL Server, and other databases to extract table data."
keywords: data extraction from database, Extract data, via JDBC, JDBC database extraction, SQLite parser Java, extract tables from database
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
---
GroupDocs.Parser provides the functionality to extract data from databases via JDBC. This feature allows you to parse database tables and extract their content as text, similar to how you would extract data from document files.

## API Overview

### Constructors

The [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) class provides constructors specifically for database extraction:

```java
// Constructors with java.sql.Connection object
Parser(java.sql.Connection connection);
Parser(java.sql.Connection connection, ParserSettings parserSettings);

// Constructor with JDBC connection string
Parser(String filePath, LoadOptions loadOptions);
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `connection` | [*java.sql.Connection*](https://docs.oracle.com/javase/8/docs/api/java/sql/Connection.html) | An open JDBC database connection |
| `filePath` | `String` | JDBC connection string (for example, `jdbc:sqlite:sample.db`) |
| `loadOptions` | [LoadOptions](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/LoadOptions) | Must be set to `FileFormat.Database` when using connection string |
| `parserSettings` | [ParserSettings](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/ParserSettings) | Optional settings for logging and other parser configurations |

### Return Types

- [getToc](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getToc()) returns `Iterable<TocItem>` where each [TocItem](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/TocItem) represents a database table: [getText](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/TocItem#getText()) returns the table name and [getPageIndex](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/TocItem#getPageIndex()) returns the table index
- [getText(int pageIndex)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getText(int)) returns [TextReader](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/TextReader) containing the table data for the specified table index; [getText()](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getText()) returns the data of all tables
- [isText](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isText()) and [isToc](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isToc()) methods of [Features](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features) class indicate if text and table extraction are supported

## Supported Databases

GroupDocs.Parser works with any database that has a JDBC driver. Add the driver to the classpath of your application. Common drivers include:

- **SQLite** - `org.xerial:sqlite-jdbc`
- **SQL Server** - `com.microsoft.sqlserver:mssql-jdbc`
- **MySQL** - `com.mysql:mysql-connector-j`
- **PostgreSQL** - `org.postgresql:postgresql`
- **Oracle** - `com.oracle.database.jdbc:ojdbc11`

## Limitations

- Database extraction is primarily for text-based table content
- Complex relationships between tables are not automatically resolved
- The list of tables is obtained from `DatabaseMetaData.getTables` with no type filter, so it contains every object the JDBC driver reports (depending on the driver, views and system tables as well)

## Extract data with Connection object

To create an instance of [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) class to extract data from a database with [*java.sql.Connection*](https://docs.oracle.com/javase/8/docs/api/java/sql/Connection.html) object the following constructors are used:

```java
// Basic constructor
Parser(java.sql.Connection connection);

// With parser settings (for logging, etc.)
Parser(java.sql.Connection connection, ParserSettings parserSettings);
```

The second constructor allows you to use [ParserSettings](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/ParserSettings) object to control the process; for example, by adding [logging functionality]({{< ref "parser/java/developer-guide/advanced-usage/logging.md" >}}).

{{< alert style="info" >}}
The connection is owned by your code: GroupDocs.Parser doesn't close it when the [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object is closed. Close the connection yourself, for example with try-with-resources.
{{< /alert >}}

### Step-by-Step Guide

Here are the steps to extract data from a database:

*  Prepare [*java.sql.Connection*](https://docs.oracle.com/javase/8/docs/api/java/sql/Connection.html) object;
*  Instantiate [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object with connection object;
*  Call [isText](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isText()) method to check if text extraction is supported;
*  Call [isToc](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isToc()) method to check if table of contents extraction is supported;
*  Call [getToc](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getToc()) method and obtain collection of tables;
*  Iterate through the collection and get a text from tables.

### Complete Example: SQLite Database

The following example shows how to extract data from a SQLite database:

```java
// Create java.sql.Connection object
try (java.sql.Connection connection = java.sql.DriverManager.getConnection(String.format("jdbc:sqlite:%s", Constants.SampleDatabase))) {
    // Create an instance of Parser class to extract tables from the database
    try (Parser parser = new Parser(connection)) {
        // Check if text extraction is supported
        if (!parser.getFeatures().isText()) {
            System.out.println("Text extraction isn't supported.");
            return;
        }
        // Check if toc extraction is supported
        if (!parser.getFeatures().isToc()) {
            System.out.println("Toc extraction isn't supported.");
            return;
        }
        // Get a list of tables
        Iterable<TocItem> toc = parser.getToc();
        // Iterate over tables
        for (TocItem i : toc) {
            // Print the table name
            System.out.println(i.getText());
            // Extract a table content as a text
            try (TextReader reader = parser.getText(i.getPageIndex())) {
                System.out.println(reader.readToEnd());
            }
        }
    }
}
```

### Example: SQL Server Database

```java
String connectionString = "jdbc:sqlserver://localhost:1433;databaseName=MyDatabase;integratedSecurity=true;";

try (java.sql.Connection connection = java.sql.DriverManager.getConnection(connectionString)) {
    try (Parser parser = new Parser(connection)) {
        if (parser.getFeatures().isText() && parser.getFeatures().isToc()) {
            for (TocItem table : parser.getToc()) {
                System.out.println("Extracting from table: " + table.getText());
                try (TextReader reader = parser.getText(table.getPageIndex())) {
                    if (reader != null) {
                        System.out.println(reader.readToEnd());
                    }
                }
            }
        }
    }
}
```

## Extract data with connection string

To create an instance of [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) class to extract data from a database with a JDBC connection string the following constructor is used:

```java
Parser(String filePath, LoadOptions loadOptions);
```

### Parameters

- **filePath**: The JDBC connection string (not a file path); it is passed to `java.sql.DriverManager.getConnection`
- **loadOptions**: Must be initialized with `FileFormat.Database`

In this case GroupDocs.Parser opens the connections itself and closes them: the connection used to list the tables is closed right after that, and the connection used to read the table data is closed when the [TextReader](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.data/TextReader) object is closed.

The list of tables is represented as table of contents. Each table can be extracted using the [getText(int)](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getText(int)) method where the integer parameter is the table index from the table of contents.

Here are the steps to extract data from Sqlite database:

*   Prepare connection string;
*   Instantiate [Parser](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser) object with connection string;
*   Call [isText](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isText()) method to check if text extraction is supported;
*   Call [isToc](https://reference.groupdocs.com/java/parser/com.groupdocs.parser.options/Features#isToc()) method to check if table of contents extraction is supported;
*   Call [getToc](https://reference.groupdocs.com/java/parser/com.groupdocs.parser/Parser#getToc()) method and obtain collection of tables;
*   Iterate through the collection and get a text from tables.

The following example shows how to extract data from Sqlite database:

```java
String connectionString = String.format("jdbc:sqlite:%s", Constants.SampleDatabase);
// Create an instance of Parser class to extract tables from the database
// As filePath connection parameters are passed; LoadOptions is set to Database file format
try (Parser parser = new Parser(connectionString, new LoadOptions(FileFormat.Database))) {
    // Check if text extraction is supported
    if (!parser.getFeatures().isText()) {
        System.out.println("Text extraction isn't supported.");
        return;
    }
    // Check if toc extraction is supported
    if (!parser.getFeatures().isToc()) {
        System.out.println("Toc extraction isn't supported.");
        return;
    }
    // Get a list of tables
    Iterable<TocItem> toc = parser.getToc();
    // Iterate over tables
    for (TocItem i : toc) {
        // Print the table name
        System.out.println(i.getText());
        // Extract a table content as a text
        try (TextReader reader = parser.getText(i.getPageIndex())) {
            System.out.println(reader.readToEnd());
        }
    }
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
