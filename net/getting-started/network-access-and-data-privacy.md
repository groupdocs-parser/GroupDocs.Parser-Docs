---
id: network-access-and-data-privacy
url: parser/net/network-access-and-data-privacy
title: Network access and data privacy
weight: 6
description: "Explains when GroupDocs.Parser for .NET accesses the network, what data leaves your machine, which extraction methods load external resources, and how to prevent outbound requests in air-gapped or security-sensitive deployments."
keywords: on-premise, offline, air-gapped, network access, data privacy, external resources, outbound requests, firewall, SSRF, tracking pixel
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
tags: csharp, parser, network, security, privacy, air-gapped
---
## Summary

GroupDocs.Parser for .NET is an **on-premise library**. It runs entirely inside your own process, on your own machine and network. It does not send your documents, your extracted data, or any customer information to GroupDocs.

As stated in the [GroupDocs Customer Data and Security policy](https://about.groupdocs.com/security/customer-data-and-security/):

> GroupDocs products run on customer's own machines, infrastructure and network and do not send any files back to GroupDocs for processing and therefore does not have access to them.

However, "we never receive your data" and "the library never opens a socket" are two different statements, and only the first is unconditionally true. GroupDocs.Parser can make outbound network requests, and one of those situations is not requested by your code at all. This topic describes each of them so that you can audit and restrict them.

{{< alert style="warning" >}}
**Short version.** GroupDocs.Parser never transmits your document content anywhere. It does, however, *retrieve* resources that a document you supply refers to — remote images and stylesheets referenced by an absolute URL. This is enabled by default.

`ExternalResourceHandler` does **not** currently suppress this on every code path. If you must guarantee that no outbound request is made, block egress at the network or process level. See [Preventing outbound requests](#preventing-outbound-requests).
{{< /alert >}}

## What never leaves your machine

Regardless of configuration, GroupDocs.Parser does **not**:

* upload documents, document fragments, or extracted text/metadata/images to GroupDocs or any third party;
* transmit file names, file paths, or directory listings;
* collect telemetry, analytics, or usage statistics about the *content* of your documents.

This matches the [GroupDocs Terms of Use](https://about.groupdocs.com/legal/terms-of-use/):

> No information is collected by the product; technical information must be provided to GroupDocs by you through the support process.

Outbound requests, where they occur, are directed either at hosts named inside the document you supplied, at hosts your own code supplies (`Parser(Uri)`, `Parser(EmailConnection)`, `Parser(DbConnection)`), or — if you use a metered license — at the licensing endpoint. Your document content is never transmitted in any of these cases.

{{< alert style="info" >}}
A file-based or stream-based license is validated entirely offline, in-process, and requires no connectivity at any point. A **metered** license is different: it reports usage volume to a billing endpoint, as described under *Metered licensing* below.
{{< /alert >}}

## When GroupDocs.Parser accesses the network

There are five situations. Four are a direct consequence of something your code chose to do. The first is implicit, and is the one most likely to surprise you.

### 1. Loading external resources referenced by a document — implicit, enabled by default

When a document references a resource by an absolute URL, GroupDocs.Parser issues an HTTP(S) request to retrieve it while building its internal document model. The following are known to trigger a request:

* `<img src="http://…">` in an HTML or MHTML file, or in the **HTML body of an email message** (measured for MSG and EML; EMLX and messages inside PST/OST are handled by the same parsers but were not measured separately);
* `<link rel="stylesheet" href="http://…">` in the same content;
* **linked (non-embedded) images** in Word processing documents;
* **`INCLUDEPICTURE` fields**, for example in RTF.

The following are known **not** to trigger a request:

* clickable hyperlinks — `<a href="http://…">` is never followed;
* `background-image` declared in inline CSS.

{{< alert style="warning" >}}
**This is the only network behaviour that is not explicitly requested by your code.** If you process untrusted documents — inbound email in particular — be aware of the consequences:

* **Tracking pixels fire.** A remote image in a marketing or phishing email confirms to the sender that the message was processed, and reveals your server's public IP address and approximate processing time.
* **Server-Side Request Forgery (SSRF).** A crafted document can name an internal host — `http://169.254.169.254/`, `http://10.0.0.5:8080/` — and cause your server to issue a request to it from inside your network perimeter.
* **Denial of service.** A document referencing a slow or unreachable host may stall extraction.

If you parse documents that arrive from outside your organisation, restrict outbound traffic from the parsing process as described in [Preventing outbound requests](#preventing-outbound-requests).
{{< /alert >}}

### 2. Parsing a document directly from a URL — explicit

The [Parser(Uri)]({{< ref "parser/net/developer-guide/advanced-usage/loading/load-document-from-url.md" >}}) constructor overloads instruct GroupDocs.Parser to download the document itself:

```csharp
using (Parser parser = new Parser(new Uri("https://example.com/sample.docx")))
{
    // ...
}
```

The download is performed by the library, but only because you supplied a `Uri`. If you never call these overloads, this path is never taken.

{{< alert style="warning" >}}
[LoadOptions.Timeout](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/loadoptions/properties/timeout) is intended to bound how long the library waits for the download. In version 26.6.1 it does not do so reliably — a server that accepts the connection and then stalls can block the constructor past the timeout. Do not depend on it as a hard limit; apply your own timeout or egress controls instead.

If the URL is derived from untrusted input, validate it against an allow-list before constructing the `Parser`. GroupDocs.Parser does not restrict schemes, hosts, ports, or private IP ranges on your behalf.
{{< /alert >}}

### 3. Connecting to a mail server or a database — explicit

The [Parser(EmailConnection)]({{< ref "parser/net/developer-guide/advanced-usage/extract-data-from-various-formats/extract-data-from-emails/extract-emails-from-remote-server-via-pop-imap-or-exchange-web-services-protocols.md" >}}) overloads connect to a POP3, IMAP, or Exchange Web Services server that **you** specify via `EmailPopConnection`, `EmailImapConnection`, or `EmailEwsConnection`. Likewise, [Parser(DbConnection)]({{< ref "parser/net/developer-guide/advanced-usage/extract-data-from-databases.md" >}}) reaches whatever server the connection you supply points at, which may be remote.

In both cases the host, port, and credentials come from your code — never from the content of a parsed document.

### 4. Metered licensing — explicit, opt-in

If — and only if — you activate a [metered license]({{< ref "parser/net/getting-started/evaluation-limitations-and-licensing.md" >}}) with `SetMeteredKey`, the library contacts the licensing and billing endpoint. The key is validated over the network when you set it, and consumption data is uploaded on a recurring basis thereafter.

What is transmitted is **usage volume** — the quantity of operations performed and total file size processed — not document content and not customer data.

{{< alert style="warning" >}}
If consumption data cannot be uploaded for more than 24 hours, the license reverts to evaluation status. **A metered license is therefore unsuitable for air-gapped deployments** — use a file-based or stream-based license, which needs no connectivity at any point.
{{< /alert >}}

### 5. Downloading a license file over HTTP — explicit, opt-in

If the `GROUPDOCS_LIC_PATH` environment variable is set to an `http://` or `https://` URL **and** the path you pass to `SetLicense` does not exist on disk, the library downloads the license from that URL. If the environment variable is unset, or the local file exists, no request is made.

## Which extraction methods load external resources

Whether a request is made depends on **both** the input format and the method you call, and there is no simple rule that spans formats. The table below records behaviour measured on **version 26.6.1** against documents referencing a remote stylesheet and a remote image; the email rows were re-confirmed on 26.7.0.

More generally, any method that has to materialise a full Word processing document model — including `GetTables()`, `GetToc()` and `GetHyperlinks()` on such input — can trigger a fetch of the external resources that document references, whether or not the method itself concerns those resources.

| Format | Method | Outbound request |
| --- | --- | :-: |
| MSG, EML | `GetText` | No |
| | `GetFormattedText` | **Yes** |
| | `GetStructure` | **Yes** |
| | `GetImages`, `GetContainer`, `GetMetadata`, `GetDocumentInfo` | No |
| HTML | `GetText`, `GetFormattedText`, `GetStructure` | **Yes** |
| | `GetImages` | **Yes** — suppressible, see below |
| | `GetDocumentInfo` | No |
| MHTML | `GetText`, `GetFormattedText`, `GetStructure` | **Yes** |
| | `GetImages` | **Yes** — suppressible, see below |
| | `GetDocumentInfo` | **Yes** |
| Word processing, with a linked image | `GetImages` | **Yes** |
| | `GetText`, `GetFormattedText`, `GetStructure` | No |
| RTF, with `INCLUDEPICTURE` | `GetText` | **Yes** |
| | `GetFormattedText`, `GetStructure`, `GetImages` | No |

One request is issued per referenced resource, per call. Results are not cached between calls, so parsing the same document twice issues the requests twice.

{{< alert style="info" >}}
`GetHyperlinks()` is frequently misunderstood. It extracts hyperlink *targets as text*: it does not resolve, follow, validate, or request them. The hyperlinks themselves are never requested — but other external resources in the same document may still be, because extracting hyperlinks from a Word processing document loads the whole document first. On HTML input the method returns `null`, and no request is made.
{{< /alert >}}

{{< alert style="warning" >}}
**`GetDocumentInfo()` is not a safe probe.** On MHTML input, asking a document for its page count is itself enough to trigger a fetch. There is no reliable "inspect first, then decide whether to parse" sequence.

Spreadsheet, presentation and PDF formats have **not** been characterised. Treat the table above as a list of confirmed cases, not as an exhaustive list of safe ones.
{{< /alert >}}

## Limits of ExternalResourceHandler

[ExternalResourceHandler]({{< ref "parser/net/developer-guide/advanced-usage/loading/handle-loading-of-external-resources.md" >}}), supplied through [ParserSettings](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/parsersettings), lets you inspect each resource URI before it is fetched and skip it by setting `args.Skipped = true`.

{{< alert style="danger" >}}
As of version 26.6.1 the handler is honoured on **image extraction from HTML and MHTML only**. On every other path — including `GetText`, `GetFormattedText` and `GetStructure`, email bodies, linked images in Word processing documents, and `INCLUDEPICTURE` fields — [OnLoading](https://reference.groupdocs.com/net/parser/groupdocs.parser.options/externalresourcehandler/methods/onloading) is never invoked and the request is still issued.

Do not rely on the handler alone to guarantee that no outbound traffic occurs. This limitation is expected to be lifted in a future release; check the release notes for the version you use.
{{< /alert >}}

Within its supported scope the handler works as documented. Use it to filter which remote images are retrieved during HTML and MHTML image extraction:

```csharp
using System.Collections.Generic;
using GroupDocs.Parser;
using GroupDocs.Parser.Data;
using GroupDocs.Parser.Options;

class DenyAllExternalResources : ExternalResourceHandler
{
    public override void OnLoading(ExternalResourceLoadingArgs args)
    {
        // Refuse every external resource, whatever its URI.
        args.Skipped = true;
    }
}

ParserSettings settings = new ParserSettings(new DenyAllExternalResources());

using (Parser parser = new Parser(filePath, settings))
{
    IEnumerable<PageImageArea> images = parser.GetImages();

    // For HTML and MHTML input, only images embedded in the document are
    // returned; externally hosted images are skipped and no request is made.
}
```

### Allowing only trusted hosts

If you need external resources from known-good locations but not from arbitrary ones, filter by URI:

```csharp
using System;
using System.Linq;
using GroupDocs.Parser.Options;

class AllowListHandler : ExternalResourceHandler
{
    private static readonly string[] AllowedPrefixes =
    {
        "https://cdn.contoso.com/",
        "https://intranet.contoso.local/assets/"
    };

    public override void OnLoading(ExternalResourceLoadingArgs args)
    {
        args.Skipped = !AllowedPrefixes.Any(prefix =>
            args.Uri.StartsWith(prefix, StringComparison.OrdinalIgnoreCase));
    }
}
```

Prefer an allow-list over a deny-list. Blocking `http://169.254.169.254/` by pattern is fragile — DNS names, redirects, IPv6 literals, and decimal-encoded IP addresses all defeat naive filtering.

## Preventing outbound requests

**Block egress at the network or process level.** Run the parsing component in a container, network namespace, VM, or subnet with no outbound route, or apply an outbound firewall rule to the process. This is the only control that covers every path, including the ones `ExternalResourceHandler` does not reach, and the only one that cannot be bypassed by a parsing bug. For air-gapped deployment this is both necessary and sufficient.

Alongside it, reduce the surface in application code:

**1. Choose a network-free method where one exists for your format.** For MSG and EML input, `GetText()` extracts the message body without any outbound request; `GetFormattedText()` and `GetStructure()` do not.

**2. Install an `ExternalResourceHandler`** that skips everything. This is effective for HTML and MHTML image extraction and harmless elsewhere, so there is no reason to omit it — but see [Limits of ExternalResourceHandler](#limits-of-externalresourcehandler) for what it does not cover.

**3. Do not use `Parser(Uri)`.** Load from a local path or a `Stream` instead. If your application must accept URLs, download them in your own code where you can apply your own validation and your own timeout, then hand the resulting `Stream` to [Parser(Stream)]({{< ref "parser/net/developer-guide/advanced-usage/loading/load-document-from-stream.md" >}}).

**4. Do not use `Parser(EmailConnection)` or `Parser(DbConnection)`** unless connecting to a mail server or a remote database is an intended feature of your application.

**5. Use a file-based or stream-based license, not a metered license**, and leave `GROUPDOCS_LIC_PATH` either unset or pointing at a local path rather than a URL.

## Diagnosing unexpected HTTP requests

If you observe outbound HTTP traffic from an application that uses GroupDocs.Parser and want to determine whether the library is responsible:

1. **Capture the requests** with Fiddler, Wireshark, or a logging proxy, and record the destination URLs.
2. **Compare them against your input documents.** If a captured URL appears verbatim inside a parsed file — as an `<img>` source, a stylesheet `href`, a linked image, or an `INCLUDEPICTURE` field — the request came from external resource loading.
3. **Re-run with outbound traffic blocked.** If parsing still produces the output you need, the requests were external resource loads and blocking egress is a complete fix.
4. **Check the destinations against the licensing endpoints** if you use a metered license or have set `GROUPDOCS_LIC_PATH` to a URL. Those requests are unrelated to the documents being parsed.

{{< alert style="info" >}}
**The request will not appear beneath your own code in a stack trace or profiler view.** External resources are fetched on an internal worker thread, not on the thread that called the extraction method. Correlate by timing, or by blocking egress and observing whether the traffic stops, rather than by looking for your call stack.
{{< /alert >}}

## More resources

### Advanced usage topics

To learn more about the features mentioned above, please refer to the following articles:

*   [Handle loading of external resources]({{< ref "parser/net/developer-guide/advanced-usage/loading/handle-loading-of-external-resources.md" >}})
*   [Load document from url]({{< ref "parser/net/developer-guide/advanced-usage/loading/load-document-from-url.md" >}})
*   [Load document from stream]({{< ref "parser/net/developer-guide/advanced-usage/loading/load-document-from-stream.md" >}})
*   [Extract data from Emails]({{< ref "parser/net/developer-guide/advanced-usage/extract-data-from-various-formats/extract-data-from-emails/_index.md" >}})
*   [Evaluation limitations and licensing]({{< ref "parser/net/getting-started/evaluation-limitations-and-licensing.md" >}})
*   [GroupDocs Customer Data and Security](https://about.groupdocs.com/security/customer-data-and-security/)

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
