---
id: network-access-and-data-privacy
url: parser/java/network-access-and-data-privacy
title: Network access and data privacy
weight: 6
description: "Explains when GroupDocs.Parser for Java accesses the network, what data leaves your machine, which extraction methods load external resources, and how to prevent outbound requests in air-gapped or security-sensitive deployments."
keywords: on-premise, offline, air-gapped, network access, data privacy, external resources, outbound requests, firewall, SSRF, tracking pixel, Java
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
tags: java, parser, network, security, privacy, air-gapped
---
## Summary

GroupDocs.Parser for Java is an **on-premise library**. It runs entirely inside your own JVM, on your own machine and network. It does not send your documents, your extracted data, or any customer information to GroupDocs.

As stated in the [GroupDocs Customer Data and Security policy](https://about.groupdocs.com/security/customer-data-and-security/):

> GroupDocs products run on customer's own machines, infrastructure and network and do not send any files back to GroupDocs for processing and therefore does not have access to them.

However, "we never receive your data" and "the library never opens a socket" are two different statements, and only the first is unconditionally true. GroupDocs.Parser can make outbound network requests, and one of those situations is not requested by your code at all. This topic describes each of them so that you can audit and restrict them.

{{< alert style="warning" >}}
**Short version.** GroupDocs.Parser never transmits your document content anywhere. It can, however, *retrieve* resources that a document you supply refers to — remote images and stylesheets referenced by an absolute URL. This is enabled by default.

`ExternalResourceHandler` does **not** currently reach every code path. If you must guarantee that no outbound request is made, block egress at the network or process level. See [Preventing outbound requests](#preventing-outbound-requests).
{{< /alert >}}

## What never leaves your machine

Regardless of configuration, GroupDocs.Parser does **not**:

* upload documents, document fragments, or extracted text/metadata/images to GroupDocs or any third party;
* transmit file names, file paths, or directory listings;
* collect telemetry, analytics, or usage statistics about the *content* of your documents.

This matches the [GroupDocs Terms of Use](https://about.groupdocs.com/legal/terms-of-use/):

> No information is collected by the product; technical information must be provided to GroupDocs by you through the support process.

Outbound requests, where they occur, are directed either at hosts named inside the document you supplied, at hosts your own code supplies (`Parser(URL)`, `Parser(EmailConnection)`, `Parser(java.sql.Connection)`), or — if you use a metered license — at the licensing endpoint. Your document content is never transmitted in any of these cases.

{{< alert style="info" >}}
A file-based or stream-based license (`License.setLicense(String)` or `License.setLicense(InputStream)`) is validated entirely offline, in-process, and requires no connectivity at any point. A **metered** license is different: it reports usage volume to a billing endpoint, as described under *Metered licensing* below.
{{< /alert >}}

## When GroupDocs.Parser accesses the network

There are four situations. Three are a direct consequence of something your code chose to do. The first is implicit, and is the one most likely to surprise you.

### 1. Loading external resources referenced by a document — implicit, enabled by default

When a document references a resource by an absolute URL, the document engine that GroupDocs.Parser uses to build its internal document model can issue an HTTP(S) request to retrieve it. The following content can trigger a request:

* `<img src="http://…">` in an HTML or MHTML file, or in the **HTML body of an email message**;
* `<link rel="stylesheet" href="http://…">` in the same content;
* **linked (non-embedded) images** in Word processing documents;
* **`INCLUDEPICTURE` fields**, for example in RTF.

Clickable hyperlinks — `<a href="http://…">` — are never followed.

Resources referenced inside EPUB and CHM e-books are resolved from the e-book file itself, not from the network.

{{< alert style="warning" >}}
**This is the only network behaviour that is not explicitly requested by your code.** If you process untrusted documents — inbound email in particular — be aware of the consequences:

* **Tracking pixels fire.** A remote image in a marketing or phishing email confirms to the sender that the message was processed, and reveals your server's public IP address and approximate processing time.
* **Server-Side Request Forgery (SSRF).** A crafted document can name an internal host — `http://169.254.169.254/`, `http://10.0.0.5:8080/` — and cause your server to issue a request to it from inside your network perimeter.
* **Denial of service.** A document referencing a slow or unreachable host may stall extraction.

If you parse documents that arrive from outside your organisation, restrict outbound traffic from the parsing process as described in [Preventing outbound requests](#preventing-outbound-requests).
{{< /alert >}}

### 2. Parsing a document directly from a URL — explicit

The [Parser(java.net.URL)]({{< ref "parser/java/developer-guide/advanced-usage/loading/load-document-from-url.md" >}}) constructor overloads instruct GroupDocs.Parser to download the document itself:

```java
URL url = new URL("https://example.com/sample.docx");

// Create an instance of Parser class with the url
try (Parser parser = new Parser(url)) {
    // ...
}
```

The download is performed by the library with `URL.openStream()`, but only because you supplied a `URL`. If you never call these overloads, this path is never taken.

{{< alert style="warning" >}}
The `URL` overloads do not apply [LoadOptions](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/loadoptions/) timeout to the download: the wait is bounded only by the JVM defaults, which for HTTP are unlimited unless you set the `sun.net.client.defaultConnectTimeout` and `sun.net.client.defaultReadTimeout` system properties. A server that accepts the connection and then stalls can block the constructor indefinitely.

`java.net.URL` accepts every scheme that the JVM supports, including `file:`, `jar:` and `ftp:`. If the URL is derived from untrusted input, validate it against an allow-list before constructing the `Parser`. GroupDocs.Parser does not restrict schemes, hosts, ports, or private IP ranges on your behalf.
{{< /alert >}}

### 3. Connecting to a mail server or a database — explicit

The [Parser(EmailConnection)]({{< ref "parser/java/developer-guide/advanced-usage/extract-data-from-various-formats/extract-data-from-emails/extract-emails-from-remote-server-via-pop-imap-or-exchange-web-services-protocols.md" >}}) overloads connect to a POP3, IMAP, or Exchange Web Services server that **you** specify via `EmailPopConnection`, `EmailImapConnection`, or `EmailEwsConnection`. Likewise, [Parser(java.sql.Connection)]({{< ref "parser/java/developer-guide/advanced-usage/extract-data-from-databases.md" >}}) reaches whatever server the JDBC connection you supply points at, which may be remote.

In both cases the host, port, and credentials come from your code — never from the content of a parsed document.

### 4. Metered licensing — explicit, opt-in

If — and only if — you activate a [metered license]({{< ref "parser/java/getting-started/evaluation-limitations-and-licensing.md" >}}) with `Metered.setMeteredKey`, the library contacts the licensing and billing endpoint (`purchase-api.dynabic.com`) over HTTPS. The key is validated over the network when you set it, and consumption data is uploaded on a recurring basis thereafter.

What is transmitted is **usage volume** — the quantity of operations performed and total file size processed — not document content and not customer data.

{{< alert style="warning" >}}
If consumption data cannot be uploaded for more than 24 hours, the metered license is reset and the product falls back to evaluation mode. **A metered license is therefore unsuitable for air-gapped deployments** — use a file-based or stream-based license, which needs no connectivity at any point.
{{< /alert >}}

{{< alert style="info" >}}
Unlike the .NET version, GroupDocs.Parser for Java never downloads a license file over HTTP: `License.setLicense` accepts only a local file path or an `InputStream`.
{{< /alert >}}

## Which extraction methods load external resources

Whether a request can be made depends on **both** the input format and the method you call. The table below shows, per format and method, whether the document is loaded into a model that can retrieve external resources, and whether your [ExternalResourceHandler]({{< ref "parser/java/developer-guide/advanced-usage/loading/handle-loading-of-external-resources.md" >}}) is consulted before a resource is retrieved.

| Format | Method | External resources retrieved | `ExternalResourceHandler` consulted |
| --- | --- | :-: | :-: |
| MSG, EML | `getText` | No | — |
| | `getFormattedText`, `getStructure` | **Yes** | **No** |
| HTML, XHTML, MHTML | `getImages`, `getTextAreas`, `getTables`, `getDocumentInfo` | **Yes** | Yes |
| | `getText`, `getFormattedText`, `getStructure` | **Yes** | **No** |
| Word processing, RTF | any method that loads the document | **Possible**, when the document has linked images or `INCLUDEPICTURE` fields | **No** |

The email rows are demonstrated by the `ExternalResourcesInEmailMessages` example in the [GitHub examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java): it watches the requests made while a message with a remote image is parsed. The other rows reflect how each format is loaded; Word processing documents are loaded without the handler, so whether a linked resource is fetched is decided by the document engine alone. Spreadsheet, presentation and PDF formats have **not** been characterised. Treat the table as a list of known cases, not as an exhaustive list of safe ones.

One request is issued per referenced resource, per load. Results are not cached between `Parser` instances, so parsing the same document twice issues the requests twice.

{{< alert style="info" >}}
`getHyperlinks()` extracts hyperlink *targets as text*: it does not resolve, follow, validate, or request them. The hyperlinks themselves are never requested — but other external resources in the same document may still be, because extracting hyperlinks from a Word processing document loads the whole document first.
{{< /alert >}}

## Limits of ExternalResourceHandler

[ExternalResourceHandler]({{< ref "parser/java/developer-guide/advanced-usage/loading/handle-loading-of-external-resources.md" >}}), supplied through [ParserSettings](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/parsersettings/), lets you inspect each resource URI before it is fetched and skip it by calling `args.setSkipped(true)`.

{{< alert style="danger" >}}
As of version 26.10 the handler is honoured only for **HTML, XHTML and MHTML** documents, and only on the methods that build the page layout: `getImages`, `getTextAreas`, `getTables` and `getDocumentInfo`. On every other path — `getText`, `getFormattedText` and `getStructure` on HTML input, email bodies, linked images in Word processing documents, and `INCLUDEPICTURE` fields — [onLoading](https://reference.groupdocs.com/parser/java/com.groupdocs.parser.options/externalresourcehandler/#onLoading-com.groupdocs.parser.options.ExternalResourceLoadingArgs-) is never invoked and the request is still issued.

Do not rely on the handler alone to guarantee that no outbound traffic occurs. Check the release notes of the version you use.
{{< /alert >}}

Within its supported scope the handler works as documented. Use it to filter which remote resources are retrieved from HTML and MHTML documents:

```java
import com.groupdocs.parser.Parser;
import com.groupdocs.parser.data.PageImageArea;
import com.groupdocs.parser.options.ExternalResourceHandler;
import com.groupdocs.parser.options.ExternalResourceLoadingArgs;
import com.groupdocs.parser.options.ParserSettings;

class DenyAllExternalResources extends ExternalResourceHandler {
    @Override
    public void onLoading(ExternalResourceLoadingArgs args) {
        // Refuse every external resource, whatever its URI
        args.setSkipped(true);
    }
}

ParserSettings settings = new ParserSettings(new DenyAllExternalResources());

try (Parser parser = new Parser(filePath, settings)) {
    Iterable<PageImageArea> images = parser.getImages();

    // For HTML and MHTML input, only images embedded in the document are
    // returned; externally hosted images are skipped and no request is made.
}
```

### Allowing only trusted hosts

If you need external resources from known-good locations but not from arbitrary ones, filter by URI:

```java
import com.groupdocs.parser.options.ExternalResourceHandler;
import com.groupdocs.parser.options.ExternalResourceLoadingArgs;

class AllowListHandler extends ExternalResourceHandler {
    private static final String[] ALLOWED_PREFIXES = {
            "https://cdn.contoso.com/",
            "https://intranet.contoso.local/assets/"
    };

    @Override
    public void onLoading(ExternalResourceLoadingArgs args) {
        String uri = args.getUri() == null ? "" : args.getUri().toLowerCase();
        boolean allowed = false;
        for (String prefix : ALLOWED_PREFIXES) {
            if (uri.startsWith(prefix)) {
                allowed = true;
                break;
            }
        }

        args.setSkipped(!allowed);
    }
}
```

Prefer an allow-list over a deny-list. Blocking `http://169.254.169.254/` by pattern is fragile — DNS names, redirects, IPv6 literals, and decimal-encoded IP addresses all defeat naive filtering.

## Preventing outbound requests

**Block egress at the network or process level.** Run the parsing component in a container, network namespace, VM, or subnet with no outbound route, or apply an outbound firewall rule to the JVM process. This is the only control that covers every path, including the ones `ExternalResourceHandler` does not reach, and the only one that cannot be bypassed by a parsing bug. For air-gapped deployment this is both necessary and sufficient.

Alongside it, reduce the surface in application code:

**1. Choose a network-free method where one exists for your format.** For MSG and EML input, `getText()` extracts the message body without any outbound request; `getFormattedText()` and `getStructure()` do not.

**2. Install an `ExternalResourceHandler`** that skips everything. This is effective for the HTML and MHTML methods listed above and harmless elsewhere, so there is no reason to omit it — but see [Limits of ExternalResourceHandler](#limits-of-externalresourcehandler) for what it does not cover.

**3. Do not use `Parser(URL)`.** Load from a local path or an `InputStream` instead. If your application must accept URLs, download them in your own code where you can apply your own validation and your own timeouts, then hand the resulting `InputStream` to [Parser(InputStream)]({{< ref "parser/java/developer-guide/advanced-usage/loading/load-document-from-stream.md" >}}).

**4. Do not use `Parser(EmailConnection)` or `Parser(java.sql.Connection)`** unless connecting to a mail server or a remote database is an intended feature of your application.

**5. Use a file-based or stream-based license, not a metered license.**

**6. Check your OCR connector.** GroupDocs.Parser for Java has no built-in OCR; OCR is performed by the [OcrConnectorBase]({{< ref "parser/java/developer-guide/advanced-usage/using-ocr/use-ocr-connector.md" >}}) implementation you supply in `ParserSettings`. If that connector calls a cloud OCR service, page images are sent to it by your connector, not by GroupDocs.Parser.

## Diagnosing unexpected HTTP requests

If you observe outbound HTTP traffic from an application that uses GroupDocs.Parser and want to determine whether the library is responsible:

1. **Capture the requests** with Fiddler, Wireshark, or a logging proxy, and record the destination URLs. Inside the JVM you can also install a `java.net.ProxySelector` that records every URI it is asked about, as the `ExternalResourcesInEmailMessages` example does.
2. **Compare them against your input documents.** If a captured URL appears verbatim inside a parsed file — as an `<img>` source, a stylesheet `href`, a linked image, or an `INCLUDEPICTURE` field — the request came from external resource loading.
3. **Re-run with outbound traffic blocked.** If parsing still produces the output you need, the requests were external resource loads and blocking egress is a complete fix.
4. **Check the destinations against the licensing endpoint** if you use a metered license. Those requests are unrelated to the documents being parsed.

## More resources

### Advanced usage topics

To learn more about the features mentioned above, please refer to the following articles:

*   [Handle loading of external resources]({{< ref "parser/java/developer-guide/advanced-usage/loading/handle-loading-of-external-resources.md" >}})
*   [Load document from url]({{< ref "parser/java/developer-guide/advanced-usage/loading/load-document-from-url.md" >}})
*   [Load document from stream]({{< ref "parser/java/developer-guide/advanced-usage/loading/load-document-from-stream.md" >}})
*   [Extract data from Emails]({{< ref "parser/java/developer-guide/advanced-usage/extract-data-from-various-formats/extract-data-from-emails/_index.md" >}})
*   [Evaluation limitations and licensing]({{< ref "parser/java/getting-started/evaluation-limitations-and-licensing.md" >}})
*   [GroupDocs Customer Data and Security](https://about.groupdocs.com/security/customer-data-and-security/)

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Parser for .NET examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET)    
*   [GroupDocs.Parser for Java examples](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)    

### Free online document parser App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to parse documents and extract data from PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, Emails and more with our free online [Free Online Document Parser App](https://products.groupdocs.app/parser).
