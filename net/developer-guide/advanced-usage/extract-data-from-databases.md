---
id: extract-data-from-databases
url: parser/net/extract-data-from-databases
title: Extract data from databases
weight: 100
version: 23.5
description: "Complete guide to extracting data from databases via ADO.NET using GroupDocs.Parser for .NET. Learn how to connect to SQLite, SQL Server, and other database providers to extract table data."
keywords: data extraction from database, Extract data, ADO.NET database extraction, SQLite parser C#, extract tables from database
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
tags: csharp, parser, database-extraction, ado.net, v23.5
---
GroupDocs.Parser provides the functionality to extract data from databases via ADO.NET. This feature allows you to parse database tables and extract their content as text, similar to how you would extract data from document files.

## API Overview

### Constructors

The `Parser` class provides constructors specifically for database extraction:

```csharp
// Constructor with DbConnection (recommended for .NET Core/.NET 5+)
Parser(DbConnection connection);
Parser(DbConnection connection, ParserSettings parserSettings);

// Constructor with connection string (only for .NET Framework)
Parser(string connectionString, LoadOptions loadOptions);
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `connection` | `DbConnection` | An active ADO.NET database connection object |
| `connectionString` | `string` | Database connection string (only supported in .NET Framework) |
| `loadOptions` | `LoadOptions` | Must be set to `FileFormat.Database` when using connection string |
| `parserSettings` | `ParserSettings` | Optional settings for logging and other parser configurations |

### Return Types

- **GetToc()** returns `IEnumerable<TocItem>` where each `TocItem` represents a database table
- **GetText(int pageIndex)** returns `TextReader` containing the table data for the specified table index
- **Features.Text** and **Features.Toc** properties indicate if text and table extraction are supported

## Supported Database Providers

GroupDocs.Parser works with any ADO.NET-compatible database provider. Common providers include:

- **SQLite** - Requires `System.Data.SQLite` NuGet package
- **SQL Server** - Built-in support via `System.Data.SqlClient`
- **MySQL** - Requires `MySql.Data` NuGet package
- **PostgreSQL** - Requires `Npgsql` NuGet package
- **Oracle** - Requires `Oracle.ManagedDataAccess` NuGet package

## Limitations

- Database extraction is primarily for text-based table content
- Complex relationships between tables are not automatically resolved
- Only table data can be extracted, not views or stored procedures
- The connection string method is only available in .NET Framework

## Extract Data with DbConnection Object (Recommended)

The `DbConnection` approach is the recommended method and works with all .NET platforms (.NET Framework, .NET Core, .NET 5+).

### Constructor Signatures

```csharp
// Basic constructor
Parser(DbConnection connection);

// With parser settings (for logging, etc.)
Parser(DbConnection connection, ParserSettings parserSettings)
```

The second constructor allows you to use [ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings) object to control the process; for example, by adding [logging functionality]({{< ref "parser/net/developer-guide/advanced-usage/logging.md" >}}).

### Step-by-Step Guide

Here are the steps to extract data from a database:

* Prepare [DbConnection](https://docs.microsoft.com/en-us/dotnet/api/system.data.common.dbconnection?view=netcore-3.1) object;
* Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object with connection string;
* Call [Features.Text](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/features/properties/text) property to check if text extraction is supported;
* Call [Features.Toc](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/features/properties/toc) property to check if table of contents extraction is supported;
* Call [GetToc](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettoc) method and obtain collection of tables;
* Iterate through the collection and get a text from tables.

### Complete Example: SQLite Database

The following example shows how to extract data from a SQLite database:

```csharp
using System.Data.Common;
using System.Data.SQLite;
using GroupDocs.Parser;
using GroupDocs.Parser.Data;

// Connection string for SQLite database
string dbPath = "sample.db";
string connectionString = $"Data Source={dbPath};Version=3;";

// Create DbConnection object
using (DbConnection connection = new SQLiteConnection(connectionString))
{
    connection.Open();
    
    // Create an instance of Parser class to extract tables from the database
    using (Parser parser = new Parser(connection))
    {
        // Check if text extraction is supported
        if (!parser.Features.Text)
        {
            Console.WriteLine("Text extraction isn't supported for this database.");
            return;
        }
        
        // Check if table of contents (table list) extraction is supported
        if (!parser.Features.Toc)
        {
            Console.WriteLine("Table of contents extraction isn't supported.");
            return;
        }
        
        // Get a list of all tables in the database
        IEnumerable<TocItem> tables = parser.GetToc();
        
        // Iterate over each table
        foreach (TocItem table in tables)
        {
            Console.WriteLine($"\n=== Table: {table.Text} ===");
            
            // Extract table content as text
            // PageIndex represents the table index
            using (TextReader reader = parser.GetText(table.PageIndex.Value))
            {
                if (reader != null)
                {
                    string tableContent = reader.ReadToEnd();
                    Console.WriteLine(tableContent);
                }
                else
                {
                    Console.WriteLine("Unable to extract data from this table.");
                }
            }
        }
    }
}
```

### Example: SQL Server Database

```csharp
using System.Data.Common;
using System.Data.SqlClient;
using GroupDocs.Parser;

string connectionString = "Server=localhost;Database=MyDatabase;Integrated Security=true;";

using (DbConnection connection = new SqlConnection(connectionString))
{
    connection.Open();
    
    using (Parser parser = new Parser(connection))
    {
        if (parser.Features.Text && parser.Features.Toc)
        {
            foreach (TocItem table in parser.GetToc())
            {
                Console.WriteLine($"Extracting from table: {table.Text}");
                using (TextReader reader = parser.GetText(table.PageIndex.Value))
                {
                    if (reader != null)
                    {
                        Console.WriteLine(reader.ReadToEnd());
                    }
                }
            }
        }
    }
}
```

## Extract Data with Connection String

{{< alert style="danger" >}}**Important:** This functionality is supported **only in .NET Framework** version of GroupDocs.Parser for .NET. For .NET Core, .NET 5+, or cross-platform applications, use the `DbConnection` approach described above.{{< /alert >}}

### Constructor

To create an instance of [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) class to extract data from a database using a connection string, use:

```csharp
Parser(string connectionString, LoadOptions loadOptions);
```

### Parameters

- **connectionString**: The database connection string (not a file path)
- **loadOptions**: Must be initialized with `FileFormat.Database`

The list of tables is represented as table of contents (TOC). Each table can be extracted using the [GetText(int)](https://reference.groupdocs.com/net/parser/groupdocs.parser.parser/gettext/methods/2) method where the integer parameter is the table index from the TOC.

Here are the steps to extract data from Sqlite database:

*   Prepare connection string;
*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object with connection string;
*   Call [Features.Text](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/features/properties/text) property to check if text extraction is supported;
*   Call [Features.Toc](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/features/properties/toc) property to check if table of contents extraction is supported;
*   Call [GetToc](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/gettoc) method and obtain collection of tables;
*   Iterate through the collection and get a text from tables.

### Complete Example (.NET Framework Only)

```csharp
using GroupDocs.Parser;
using GroupDocs.Parser.Options;

string databasePath = "sample.db";
string connectionString = string.Format(
    "Provider=System.Data.Sqlite;Data Source={0};Version=3;", 
    databasePath
);

// Create an instance of Parser class to extract tables from the database
// Pass connection string as first parameter and LoadOptions with FileFormat.Database
using (Parser parser = new Parser(connectionString, new LoadOptions(FileFormat.Database)))
{
    // Check if text extraction is supported
    if (!parser.Features.Text)
    {
        Console.WriteLine("Text extraction isn't supported.");
        return;
    }
    
    // Check if table of contents extraction is supported
    if (!parser.Features.Toc)
    {
        Console.WriteLine("Table of contents extraction isn't supported.");
        return;
    }
    
    // Get a list of all tables in the database
    IEnumerable<TocItem> tables = parser.GetToc();
    
    // Iterate over tables
    foreach (TocItem table in tables)
    {
        // Print the table name
        Console.WriteLine($"\nTable: {table.Text}");
        
        // Extract table content as text using the table's page index
        using (TextReader reader = parser.GetText(table.PageIndex.Value))
        {
            if (reader != null)
            {
                Console.WriteLine(reader.ReadToEnd());
            }
        }
    }
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).