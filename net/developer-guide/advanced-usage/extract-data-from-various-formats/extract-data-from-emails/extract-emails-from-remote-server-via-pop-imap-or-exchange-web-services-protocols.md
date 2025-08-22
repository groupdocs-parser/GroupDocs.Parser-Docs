---
id: extract-emails-from-remote-server-via-pop-imap-or-exchange-web-services-protocols
url: parser/net/extract-emails-from-remote-server-via-pop-imap-or-exchange-web-services-protocols
title: Extract emails from remote server via POP IMAP or Exchange Web Services protocols
weight: 6
description: "GroupDocs.Parser allows you to extract emails from remote servers and data from the emails. It supports POP, IMAP and EWS protocols."
keywords: Extract emails, Extract emails from remote server, POP, IMAP, EWS
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
---
GroupDocs.Parser allows you to extract emails from remote servers and data from the emails. The following email protocols are supported:

*   Post Office Protocol (POP)
*   Internet Message Access Protocol (IMAP)
*   Exchange Web Services (EWS)

To create an instance of [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) class to extract emails from a remote server the following constructors are used:

```csharp
Parser(EmailConnection connection);
Parser(EmailConnection connection, ParserSettings parserSettings);
```

The second constructor allows to use [ParserSettings](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/parsersettings) object to control the process; for example, by adding [logging functionality]({{< ref "parser/net/developer-guide/advanced-usage/logging.md" >}}).

[EmailConnection](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/emailconnection) is a base class. The following connection classes are used:

| Protocol | Class |
| --- | --- |
| POP | [EmailPopConnection](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/emailpopconnection) |
| IMAP | [EmailImapConnection](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/emailimapconnection) |
| Exchange Web Services | [EmailEwsConnection](https://reference.groupdocs.com/parser/net/groupdocs.parser.options/emailewsconnection) |

Here are the steps to extract emails from the remote server:

*   Prepare connection string (see table below);
*   Instantiate [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object with connection string;
*   Call [Features.Container](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/features/properties/container) property to check if container extraction is supported;
*   Call [GetContainer](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser/methods/getcontainer) method and obtain collection of document container item objects;
*   Iterate through the collection and get [Parser](https://reference.groupdocs.com/net/parser/groupdocs.parser/parser) object for each item.

The following example shows how to extract emails from Exchange Server:

```csharp
// Create the connection object for Exchange Web Services protocol 
EmailConnection connection = new EmailEwsConnection(
    "https://outlook.office365.com/ews/exchange.asmx",
    "email@server",
    "password");
 
// Create an instance of Parser class to extract emails from the remote server
using (Parser parser = new Parser(connection))
{
    // Check if container extraction is supported
    if (!parser.Features.Container)
    {
        Console.WriteLine("Container extraction isn't supported.");
        return;
    }
 
    // Extract email messages from the server
    IEnumerable<ContainerItem> emails = parser.GetContainer();
 
    // Iterate over attachments
    foreach (ContainerItem item in emails)
    {
        // Create an instance of Parser class for email message
        using (Parser emailParser = item.OpenParser())
        {
            // Extract the email text
            using (TextReader reader = emailParser.GetText())
            {
                // Print the email text
                Console.WriteLine(reader == null ? "Text extraction isn't supported." : reader.ReadToEnd());
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