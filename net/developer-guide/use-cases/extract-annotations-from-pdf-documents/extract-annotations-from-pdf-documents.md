---
id: extract-annotations-from-pdf-documents
url: parser/net/extract-annotations-from-pdf-documents-use-case
title: "Extract Annotations from PDF Documents"
description: "Step‑by‑step guide to extracting PDF annotations in .NET using GroupDocs.Parser, with per‑page extraction, combined text output, and export options."
keywords:
  - GroupDocs.Parser
  - PDF annotations
  - .NET
  - document review
  - PDF parsing
  - CSV export
  - JSON export
productName: "GroupDocs.Parser for .NET"
weight: 20
toc: true
hideChildren: false
draft: false
---

{{< alert style="info" >}}
💡 Full working example available on GitHub:
[extract-annotations-from-pdf-documents-using-groupdocs-parser-dotnet](https://github.com/groupdocs-parser/extract-annotations-from-pdf-using-groupdocs-parser-dotnet/)
{{< /alert >}}

**Extract Annotations from PDF Documents** is a GroupDocs.Parser capability for .NET that reads reviewer comments, sticky notes, and other markup left on a PDF file and returns them as structured data. The guide below shows how to turn that markup into something you can actually use, whether you need a flat dump of every comment, a page‑by‑page breakdown, or a combined text‑plus‑annotations transcript.

---

## Business Scenario: Collecting Reviewer Feedback

Publishing and legal teams routinely pass a PDF through several reviewers before it's finalized. Each reviewer leaves comments directly in the file instead of writing a separate summary, which means the feedback is easy to miss unless someone opens every page in a PDF viewer. Pulling the annotations out programmatically turns that scattered feedback into a checklist the editor can work through.

I ran into this while building a lightweight review tracker for a documentation team: a 40‑page release note went through three reviewers, and manually scrolling through it to find every sticky note took longer than actually addressing the feedback. Extracting the annotations in a few lines of code turned that into a two‑minute job.

---

## Why PDF Annotations Matter

Annotations are the only record of *who* asked for a change and *why*, since the final rendered PDF usually hides that context once comments are resolved or the file is flattened. Reading them programmatically lets you:

- Build a checklist of open review comments without opening the file.
- Archive reviewer feedback alongside the document for future audits.
- Feed comments into a ticketing system automatically.

GroupDocs.Parser added native annotation extraction for PDF documents in version 26.7 via the [GetAnnotations](https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/getannotations/) method — see the [26.7 release notes](https://releases.groupdocs.com/parser/net/release-notes/2026/groupdocs-parser-for-net-26-7-release-notes/) for the full API change list.

---

## Prerequisites

- .NET 6.0 SDK or later installed on your machine.
- A temporary license key (valid for 30 days) from the GroupDocs portal [Temp License][TEMP_LICENSE].
- GroupDocs.Parser for .NET 26.7 or later.
- A PDF with existing annotations, e.g., `document-with-annotations.pdf`.

## Installation

```bash
# Add the GroupDocs.Parser NuGet package to your project
dotnet add package GroupDocs.Parser
```

## Repository Overview

The sample repository follows a clean, modular layout:

```
extract-annotations-from-pdf-documents-net/
│
├── Methods
│   ├── CheckAnnotationSupport.cs        # Feature-support check
│   ├── ExtractAllAnnotations.cs         # Whole-document extraction
│   ├── ExtractAnnotationsByPage.cs      # Page-by-page extraction
│   ├── ExtractTextWithAnnotations.cs    # Combined text + annotations
│   ├── ExportAnnotationsToCsv.cs        # CSV exporter
│   └── ExportAnnotationsToJson.cs       # JSON exporter
├── resources
│   └── document-with-annotations.pdf
├── ExtractAnnotationsFromPdf.csproj
└── Program.cs                          # Orchestrates the workflow
```

Each method is deliberately small (under 25 lines) so you can copy‑paste it into your own project without pulling in unnecessary dependencies.

---

## Step‑by‑Step Implementation

### 1. Check Annotation Support (Foundation)

Not every file format supports annotations, and `GetAnnotations` returns `null` when it doesn't. Checking `Features.Annotations` upfront lets you fail fast with a clear message instead of guarding every call with a null check.

```csharp
using (var parser = new Parser(path))
{
    return parser.Features.Annotations;
}
```

**How it works:** `Parser.Features` exposes a `Boolean Annotations` flag that reflects whether the loaded document format supports annotation extraction.

---

### 2. Extract All Annotations (Whole‑Document Dump)

Use the parameterless `GetAnnotations()` overload when you just need every comment in the file, regardless of which page it's on.

```csharp
var result = new List<string>();
using (var parser = new Parser(path))
{
    IEnumerable<AnnotationItem> annotations = parser.GetAnnotations();
    if (annotations == null)
    {
        return result;
    }

    foreach (var item in annotations)
    {
        result.Add(item.Value);
    }
}
return result;
```

**Key points:**
- `GetAnnotations()` returns `null` when annotation extraction isn't supported, and an empty collection when the document simply has no annotations — the two cases are distinguishable.
- Each `AnnotationItem` exposes its text through the `Value` property.
- Best for a quick "does this file have open comments at all?" check.

---

### 3. Extract Annotations Page‑by‑Page (Where Was It Left?)

When you need to know which page a comment belongs to, call `GetAnnotations(pageIndex)` in a loop driven by `GetDocumentInfo().PageCount`.

```csharp
var result = new List<AnnotationRecord>();
using (var parser = new Parser(path))
{
    if (!parser.Features.Annotations)
    {
        return result;
    }

    var info = parser.GetDocumentInfo();
    if (info == null || info.PageCount == 0)
    {
        return result;
    }

    for (int pageIndex = 0; pageIndex < info.PageCount; pageIndex++)
    {
        IEnumerable<AnnotationItem> pageAnnotations = parser.GetAnnotations(pageIndex);
        if (pageAnnotations == null)
        {
            continue;
        }

        foreach (var item in pageAnnotations)
        {
            result.Add(new AnnotationRecord { PageIndex = pageIndex, Value = item.Value });
        }
    }
}
return result;
```

**Why use it:** The output is a flat list of *page → comment* pairs, which is exactly the shape a CSV or JSON export needs.

---

### 4. Extract Document Text Together with Annotations

Sometimes it's more useful to see a comment in the flow of the surrounding text rather than as a separate list. Setting `IncludeAnnotations` on `TextOptions` folds annotation text into the regular `GetText` output.

```csharp
using (var parser = new Parser(path))
{
    var options = new TextOptions
    {
        IncludeAnnotations = true
    };

    using (TextReader reader = parser.GetText(options))
    {
        return reader?.ReadToEnd() ?? string.Empty;
    }
}
```

**When it shines:** Generating a single transcript‑style document that a reviewer can read top to bottom, comments and all, without cross‑referencing a separate list.

---

## Exporting the Annotations

### How can I export extracted annotations to CSV?

A CSV export lets reviewers open the comment list directly in Excel or feed it into a tracking spreadsheet. The exporter iterates over the page‑tagged records built in step 3 and writes a two‑column file (`page,value`).

```csharp
var sb = new StringBuilder();
sb.AppendLine("page,value");

foreach (var record in records)
{
    sb.AppendLine($"{record.PageIndex},{CsvEscape(record.Value)}");
}

File.WriteAllText(outputPath, sb.ToString());
```

### How do I export the annotations to JSON for a pipeline?

JSON is a better fit when a downstream service needs to consume the comments directly, without parsing a spreadsheet. The exporter builds a simple array of `{ page, value }` objects.

```csharp
var sb = new StringBuilder();
sb.AppendLine("[");

for (int i = 0; i < records.Count; i++)
{
    var comma = i < records.Count - 1 ? "," : string.Empty;
    sb.AppendLine($"  {{ \"page\": {records[i].PageIndex}, \"value\": \"{Escape(records[i].Value)}\" }}{comma}");
}

sb.AppendLine("]");
File.WriteAllText(outputPath, sb.ToString());
```

Both exporters can be called immediately after `ExtractAnnotationsByPage.Run` completes.

---

## Choosing the Right Extraction Strategy

| Strategy | When to Use | Output Shape | Typical Audience |
|----------|--------------|--------------|-------------------|
| **Whole‑Document Extraction** | Quick check for any open comments | Flat list of comment text | Editors doing a first pass |
| **Page‑by‑Page Extraction** | You need to know where each comment sits | List of page + comment pairs | Reviewers, ticketing pipelines |
| **Combined Text + Annotations** | You want a single readable transcript | One text stream | Anyone reading the document end to end |

Start with the whole‑document extraction to confirm a file has comments worth acting on, then switch to page‑by‑page extraction once you need to route feedback to the right section.

---

## Common Pitfalls & How to Avoid Them

- **Treating `null` and an empty collection the same way** – `GetAnnotations` returns `null` when the format doesn't support annotations, and an empty `IEnumerable` when it does but the document has none. Logging these differently saves confusing debugging sessions later.
- **Skipping the feature check** – Calling `GetAnnotations` on a format without annotation support is safe (it just returns `null`), but checking `Features.Annotations` first makes the intent of your code explicit.
- **Forgetting `using` blocks** – `Parser` holds native resources; always dispose it via a `using` block, especially when extracting from many files in a batch.
- **Assuming annotation extraction works for every format** – as of GroupDocs.Parser 26.7 this feature targets PDF documents; support for other formats depends on what the underlying format allows.
- **Mixing whole‑document and per‑page results** – `GetAnnotations()` doesn't report a page number, so don't try to reconcile it with the page‑tagged list; pick one extraction strategy per use case.

---

## Tips & Notes

- Wrap every `Parser` usage in a `using` block to release file handles promptly.
- Re‑use the page‑tagged records from `ExtractAnnotationsByPage` as the single source of truth for both CSV and JSON exports, so the two outputs never drift apart.
- Store the temporary license key in an environment variable rather than hard‑coding it.
- For large batches, parallelize extraction across files but keep the export step sequential to avoid file‑write collisions.
- Combine annotation extraction with `GetText` in raw mode when you only need a fast sanity check on comment volume.

---

## Frequently Asked Questions

### Q: What happens if the PDF has no annotations at all?
**A:** `GetAnnotations()` returns an empty collection, not `null`. Your iteration code runs normally and simply produces zero results — no extra null‑checking is required beyond the initial support check.

### Q: Can I extract annotations from a password‑protected PDF?
**A:** Yes, as long as you supply the correct password when loading the document, the same way you would for any other extraction operation with `GroupDocs.Parser`. Annotation extraction works on the decrypted content once the `Parser` object is created successfully.

### Q: Does `GetAnnotations` tell me who left the comment or when?
**A:** No. `AnnotationItem` currently exposes only the annotation's text through the `Value` property. If you need reviewer identity or timestamps, you'll need to parse that information from the annotation text itself if the reviewer included it there.

### Q: Can I get the annotation text mixed into the document text automatically?
**A:** Yes — set `IncludeAnnotations = true` on `TextOptions` and pass it to `GetText`. This avoids a second pass over the document when you just want one combined transcript.

---

## Conclusion

GroupDocs.Parser for .NET gives you a direct, programmatic way to pull reviewer comments out of a PDF instead of scrolling through the file by hand. Whether you need a quick yes/no check, a page‑tagged report, or a combined text‑and‑annotations transcript, the sample methods above cover the most common review workflows. Export the results to CSV or JSON, wire them into your ticketing system, and stop losing feedback in a PDF nobody re‑opens.

Ready to integrate? Review the full source code on GitHub, adapt the snippets to your project structure, and start turning PDF comments into actionable checklists today.

---

## See Also

- [Extract Annotations from PDF Documents in C# .NET][DOCS_URL]
- [GetAnnotations API Reference][API_REF_URL]
- [GroupDocs.Parser for .NET 26.7 Release Notes][RELEASE_NOTES_URL]
- [Temporary License Request][TEMP_LICENSE]
- [Sample Projects Repository][EXAMPLES_REPO]
- [Parser Blog Category][BLOG_CATEGORY]

[DOCS_URL]: https://docs.groupdocs.com/parser/net/extract-annotations-from-pdf-documents/
[API_REF_URL]: https://reference.groupdocs.com/parser/net/groupdocs.parser/parser/getannotations/
[RELEASE_NOTES_URL]: https://releases.groupdocs.com/parser/net/release-notes/2026/groupdocs-parser-for-net-26-7-release-notes/
[TEMP_LICENSE]: https://purchase.groupdocs.com/temporary-license/
[EXAMPLES_REPO]: https://github.com/groupdocs-parser/GroupDocs.Parser-for-.NET
[BLOG_CATEGORY]: https://blog.groupdocs.com/categories/groupdocs.parser-product-family/
