---
id: extract-tables-from-document-page
url: parser/net/extract-tables-from-document-page
title: Extract tables from document page
weight: 4
description: "This article explains that how to extract tables from document page."
keywords: extract tables, extract tables from document page
productName: GroupDocs.Parser for .NET
hideChildren: False
---

GroupDocs.Parser provides the functionality to extract tables from document page by the [GetTables(Int32, PageTableAreaOptions)](https://reference.groupdocs.com/parser/net/groupdocs.parser.parser/gettables/methods/1) method:

```csharp
IEnumerable<PageTableArea> GetTables(int pageIndex, PageTableAreaOptions options);
```

This method returns a collection of [PageTableArea](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagetablearea) object:

| Member                                                       | Description                                     |
| ------------------------------------------------------------ | ----------------------------------------------- |
| [Rectangle](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagearea/properties/rectangle) | The rectangular area that bounds text area.     |
| [Page](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagearea/properties/page) | The page information (page index and page size) |
| [RowCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagetablearea/properties/rowcount) | The total number of the table rows.             |
| [ColumnCount](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagetablearea/properties/columncount) | The total number of the table columns.          |
| PageTableAreaCell [Item](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagetablearea/properties/item) | The table cell by row and column indexes.       |
| double [GetRowHeight(int)](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagetablearea/methods/getrowheight) | The the row height.                             |
| double [GetColumnWidth(int)](https://reference.groupdocs.com/net/parser/groupdocs.parser.data/pagetablearea/methods/getcolumnwidth) | Returns the column width.                       |

[GetTables(Int32, PageTableAreaOptions)](https://reference.groupdocs.com/parser/net/groupdocs.parser.parser/gettables/methods/1) accepts [PageTableAreaOptions](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/pagetableareaoptions) object that contains [TemplateTableLayout](https://reference.groupdocs.com/parser/net/groupdocs.parser.templates/templatetablelayout) object with table layout (see [this article]({{< ref "parser/net/developer-guide/advanced-usage/working-with-templates.md" >}}) for more details).

Here are the steps to extract tables from the document page:

- Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for the initial document;
- Check if the document supports hyperlink extraction;
- Call [GetTables(Int32, PageTableAreaOptions)](https://reference.groupdocs.com/parser/net/groupdocs.parser.parser/gettables/methods/1) method with page index and obtain collection of [PageTableArea](https://reference.groupdocs.com/parser/net/groupdocs.parser.data/pagetablearea) objects;
- Iterate through the collection and print table cells.

The following example shows how to extract tables from the document page:

```csharp
// Create an instance of Parser class
using (Parser parser = new Parser(filePath))
{
    // Check if the document supports table extraction
    if (!parser.Features.Tables)
    {
        Console.WriteLine("Document isn't supports tables extraction.");
        return;
    }
    // Create the layout of tables
    TemplateTableLayout layout = new TemplateTableLayout(
        new double[] { 50, 95, 275, 415, 485, 545 },
        new double[] { 325, 340, 365, 395 });
    // Create the options for table extraction
    PageTableAreaOptions options = new PageTableAreaOptions(layout);
    // Get the document info
    IDocumentInfo documentInfo = parser.GetDocumentInfo();
    // Check if the document has pages
    if (documentInfo.PageCount == 0)
    {
        Console.WriteLine("Document hasn't pages.");
        return;
    }
    // Iterate over pages
    for (int pageIndex = 0; pageIndex < documentInfo.PageCount; pageIndex++)
    {
        // Print a page number 
        Console.WriteLine(string.Format("Page {0}/{1}", pageIndex + 1, documentInfo.PageCount));
        // Extract tables from the document page
        IEnumerable<PageTableArea> tables = parser.GetTables(pageIndex, options);
        // Iterate over tables
        foreach (PageTableArea t in tables)
        {
            // Iterate over rows
            for (int row = 0; row < t.RowCount; row++)
            {
                // Iterate over columns
                for (int column = 0; column < t.ColumnCount; column++)
                {
                    // Get the table cell
                    PageTableAreaCell cell = t[row, column];
                    if (cell != null)
                    {
                        // Print the table cell text
                        Console.Write(cell.Text);
                        Console.Write(" | ");
                    }
                }
                Console.WriteLine();
            }
            Console.WriteLine();
        }
    }
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

- [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)
- [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)

### Free online image extractor App

Along with full featured .NET library we provide simple, but powerfull free APPs.

You are welcome to extract images from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [GroupDocs Parser App](https://products.groupdocs.app/parser).