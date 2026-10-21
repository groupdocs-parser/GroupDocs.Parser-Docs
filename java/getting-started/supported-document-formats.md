---
id: supported-document-formats
url: parser/java/supported-document-formats
title: Supported Document Formats
weight: 2
description: ""
keywords: 
productName: GroupDocs.Parser for Java
hideChildren: False
---

## 65+ Supported File Formats
GroupDocs.Parser for Java supports **65+ file formats**. Use the search box below to quickly find your format, or browse by category.

<div style="margin: 20px 0; padding: 15px; background: #f8f9fa; border-radius: 8px; border-left: 4px solid #007bff;">
  <div style="margin-bottom: 15px;">
    <input type="text" id="formatSearch" placeholder="🔍 Search for a file format (e.g., docx, pdf, png)..." 
           style="width: 100%; padding: 10px; border: 2px solid #ddd; border-radius: 6px; font-size: 16px;">
  </div>
  <div id="searchResults" style="display: none; margin-top: 15px; padding: 10px; background: white; border-radius: 6px; border: 1px solid #ddd;">
    <strong>Search Results:</strong>
    <div id="resultsList"></div>
  </div>
</div>

## 🚀 Quick Format Lookup

Each format may support different metadata capabilities:
- ✅ Supported
- ⚠️ Partially supported
- ❌ Not supported

---

## Archive Formats

| Format | Description | ExtractContainersAndAttachments | ExtractImages | ExtractMetadata | ExtractStructuredAndFormattedText | ExtractTableOfContents | ExtractTextAccurate | ExtractTextAreas | ExtractTextRaw | ParseDocumentByTemplate | ParseFormData | ScanBarcode | Remarks |
|--------|-------------|------|------|------|------|------|------|------|------|------|------|------|---------|
| `.7z` | 7z Archive File | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | Encrypted 7-zip archives are not supported |
| `.bz2` | Bzip2 Archive File | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.gz` | Gzip Archive File | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.rar` | RAR Archive File | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.tar` | Tar Archive File | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.zip` | Zipped File | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |  |

---

## Ebook Formats

| Format | Description | ExtractContainersAndAttachments | ExtractImages | ExtractMetadata | ExtractStructuredAndFormattedText | ExtractTableOfContents | ExtractTextAccurate | ExtractTextAreas | ExtractTextRaw | ParseDocumentByTemplate | ParseFormData | ScanBarcode | Remarks |
|--------|-------------|------|------|------|------|------|------|------|------|------|------|------|---------|
| `.azw3` | AZW3 | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.chm` | Compiled HTML Help File | ❌ | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.epub` | Open eBook File | ❌ | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.fb2` | FictionBook | ❌ | ✅ | ❌ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.mobi` | FictionBook | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |  |

---

## Email Formats

| Format | Description | ExtractContainersAndAttachments | ExtractImages | ExtractMetadata | ExtractStructuredAndFormattedText | ExtractTableOfContents | ExtractTextAccurate | ExtractTextAreas | ExtractTextRaw | ParseDocumentByTemplate | ParseFormData | ScanBarcode | Remarks |
|--------|-------------|------|------|------|------|------|------|------|------|------|------|------|---------|
| `.eml` | E-Mail Message | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.emlx` | Apple Mail Message | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.msg` | Outlook Mail Message | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.ost` | Outlook Offline Data File | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.pst` | Outlook Personal Information Store File | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |  |

---

## Image Formats

| Format | Description | ExtractContainersAndAttachments | ExtractImages | ExtractMetadata | ExtractStructuredAndFormattedText | ExtractTableOfContents | ExtractTextAccurate | ExtractTextAreas | ExtractTextRaw | ParseDocumentByTemplate | ParseFormData | ScanBarcode | Remarks |
|--------|-------------|------|------|------|------|------|------|------|------|------|------|------|---------|
| `.bmp` | Bitmap Image File | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.dcm` | DICOM Image | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.djvu` | DjVu Image | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.emf` | Enhanced Windows Metafile | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.gif` | Graphical Interchange Format File | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.j2k` | JPEG 2000 Code Stream | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.jp2` | JPEG 2000 Core Image File | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.jpeg` | JPEG Image | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.jpg` | JPEG Image | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.png` | Portable Network Graphic | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.ps` | PostScript File | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.psd` | Adobe Photoshop Document | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.svg` | Scalable Vector Graphics File | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.svgz` | Compressed Scalable Vector Graphics File | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.tif` | Tagged Image File | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.tiff` | Tagged Image File Format | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.webp` | WebP Image | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |  |
| `.wmf` | Windows Metafile | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |  |

---

## Markup Formats

| Format | Description | ExtractContainersAndAttachments | ExtractImages | ExtractMetadata | ExtractStructuredAndFormattedText | ExtractTableOfContents | ExtractTextAccurate | ExtractTextAreas | ExtractTextRaw | ParseDocumentByTemplate | ParseFormData | ScanBarcode | Remarks |
|--------|-------------|------|------|------|------|------|------|------|------|------|------|------|---------|
| `.md` | Markdown Files | ❌ | ❌ | ❌ | ⚠️ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | Formatted Text is not supported |
| `.mht` | MHTML Web Archive | ❌ | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.mhtml` | MIME HTML File | ❌ | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.xhtml` | Hypertext Markup Language File | ❌ | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.xml` | XML File | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |  |

---

## Note Formats

| Format | Description | ExtractContainersAndAttachments | ExtractImages | ExtractMetadata | ExtractStructuredAndFormattedText | ExtractTableOfContents | ExtractTextAccurate | ExtractTextAreas | ExtractTextRaw | ParseDocumentByTemplate | ParseFormData | ScanBarcode | Remarks |
|--------|-------------|------|------|------|------|------|------|------|------|------|------|------|---------|
| `.one` | OneNote Document | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |  |

---

## Pdf Formats

| Format | Description | ExtractContainersAndAttachments | ExtractImages | ExtractMetadata | ExtractStructuredAndFormattedText | ExtractTableOfContents | ExtractTextAccurate | ExtractTextAreas | ExtractTextRaw | ParseDocumentByTemplate | ParseFormData | ScanBarcode | Remarks |
|--------|-------------|------|------|------|------|------|------|------|------|------|------|------|---------|
| `.pdf` | Portable Document Format File | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |  |

---

## Presentation Formats

| Format | Description | ExtractContainersAndAttachments | ExtractImages | ExtractMetadata | ExtractStructuredAndFormattedText | ExtractTableOfContents | ExtractTextAccurate | ExtractTextAreas | ExtractTextRaw | ParseDocumentByTemplate | ParseFormData | ScanBarcode | Remarks |
|--------|-------------|------|------|------|------|------|------|------|------|------|------|------|---------|
| `.odp` | OpenDocument Presentation | ❌ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ |  |
| `.otp` | OpenDocument Presentation Template | ❌ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ |  |
| `.pot` | PowerPoint Template | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |  |
| `.potm` | PowerPoint Open XML Macro-Enabled Presentation Template | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |  |
| `.potx` | PowerPoint Open XML Presentation Template | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |  |
| `.pps` | PowerPoint Slide Show | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |  |
| `.ppsm` | PowerPoint Open XML Macro-Enabled Slide | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |  |
| `.ppsx` | PowerPoint Open XML Slide Show | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |  |
| `.ppt` | PowerPoint Presentation | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |  |
| `.pptm` | PowerPoint Open XML Macro-Enabled Presentation | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |  |
| `.pptx` | PowerPoint Open XML Presentation | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |  |

---

## Spreadsheet Formats

| Format | Description | ExtractContainersAndAttachments | ExtractImages | ExtractMetadata | ExtractStructuredAndFormattedText | ExtractTableOfContents | ExtractTextAccurate | ExtractTextAreas | ExtractTextRaw | ParseDocumentByTemplate | ParseFormData | ScanBarcode | Remarks |
|--------|-------------|------|------|------|------|------|------|------|------|------|------|------|---------|
| `.csv` | Comma Separated Values File | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |  |
| `.numbers` | Apple iWork Numbers | ❌ | ✅ | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ |  |
| `.ods` | OpenDocument Spreadsheet | ❌ | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ |  |
| `.ots` | OpenDocument Spreadsheet Template | ❌ | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ |  |
| `.xla` | Excel Add-In File | ❌ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |  |
| `.xlam` | Excel Open XML Macro-Enabled Add-In | ❌ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |  |
| `.xls` | Excel Spreadsheet | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |  |
| `.xlsb` | Excel Binary Spreadsheet | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ |  |
| `.xlsm` | Excel Open XML Macro-Enabled Spreadsheet | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |  |
| `.xlsx` | Microsoft Excel Open XML Spreadsheet | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |  |
| `.xlt` | Microsoft Excel Template | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |  |
| `.xltm` | Excel Open XML Macro-Enabled Spreadsheet Template | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |  |
| `.xltx` | Excel Open XML Spreadsheet Template | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |  |

---

## WordProcessing Formats

| Format | Description | ExtractContainersAndAttachments | ExtractImages | ExtractMetadata | ExtractStructuredAndFormattedText | ExtractTableOfContents | ExtractTextAccurate | ExtractTextAreas | ExtractTextRaw | ParseDocumentByTemplate | ParseFormData | ScanBarcode | Remarks |
|--------|-------------|------|------|------|------|------|------|------|------|------|------|------|---------|
| `.doc` | Microsoft Word Document | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ |  |
| `.docm` | Word Open XML Macro-Enabled Document | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ |  |
| `.docx` | Microsoft Word Open XML Document | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ |  |
| `.dot` | Word Document Template | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ |  |
| `.dotm` | Word Open XML Macro-Enabled Document Template | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ |  |
| `.dotx` | Word Open XML Document Template | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ |  |
| `.odt` | OpenDocument Text Document | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ |  |
| `.ott` | OpenDocument Document Template | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ |  |
| `.rtf` | Rich Text Format File | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ |  |
| `.txt` | Plain Text File | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |  |

---

<script>
document.addEventListener('DOMContentLoaded', function() {
    const searchInput = document.getElementById('formatSearch');
    const searchResults = document.getElementById('searchResults');
    const resultsList = document.getElementById('resultsList');

    const formats = [
  {
    "ext": ".7z",
    "name": "7z Archive File",
    "category": "Archive",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".bz2",
    "name": "Bzip2 Archive File",
    "category": "Archive",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".gz",
    "name": "Gzip Archive File",
    "category": "Archive",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".rar",
    "name": "RAR Archive File",
    "category": "Archive",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".tar",
    "name": "Tar Archive File",
    "category": "Archive",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".zip",
    "name": "Zipped File",
    "category": "Archive",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".azw3",
    "name": "AZW3",
    "category": "Ebook",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".chm",
    "name": "Compiled HTML Help File",
    "category": "Ebook",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "✅",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ✅ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".epub",
    "name": "Open eBook File",
    "category": "Ebook",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "✅",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ✅ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".fb2",
    "name": "FictionBook",
    "category": "Ebook",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".mobi",
    "name": "FictionBook",
    "category": "Ebook",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".eml",
    "name": "E-Mail Message",
    "category": "Email",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".emlx",
    "name": "Apple Mail Message",
    "category": "Email",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".msg",
    "name": "Outlook Mail Message",
    "category": "Email",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".ost",
    "name": "Outlook Offline Data File",
    "category": "Email",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".pst",
    "name": "Outlook Personal Information Store File",
    "category": "Email",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".bmp",
    "name": "Bitmap Image File",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".dcm",
    "name": "DICOM Image",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".djvu",
    "name": "DjVu Image",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".emf",
    "name": "Enhanced Windows Metafile",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".gif",
    "name": "Graphical Interchange Format File",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".j2k",
    "name": "JPEG 2000 Code Stream",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".jp2",
    "name": "JPEG 2000 Core Image File",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".jpeg",
    "name": "JPEG Image",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".jpg",
    "name": "JPEG Image",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".png",
    "name": "Portable Network Graphic",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".ps",
    "name": "PostScript File",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".psd",
    "name": "Adobe Photoshop Document",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".svg",
    "name": "Scalable Vector Graphics File",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".svgz",
    "name": "Compressed Scalable Vector Graphics File",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".tif",
    "name": "Tagged Image File",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".tiff",
    "name": "Tagged Image File Format",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".webp",
    "name": "WebP Image",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".wmf",
    "name": "Windows Metafile",
    "category": "Image",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "❌",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ❌ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".md",
    "name": "Markdown Files",
    "category": "Markup",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "⚠️",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ⚠️ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".mht",
    "name": "MHTML Web Archive",
    "category": "Markup",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".mhtml",
    "name": "MIME HTML File",
    "category": "Markup",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".xhtml",
    "name": "Hypertext Markup Language File",
    "category": "Markup",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".xml",
    "name": "XML File",
    "category": "Markup",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".one",
    "name": "OneNote Document",
    "category": "Note",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".pdf",
    "name": "Portable Document Format File",
    "category": "Pdf",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "✅",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "✅",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ✅ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ✅ | ScanBarcode: ✅"
  },
  {
    "ext": ".odp",
    "name": "OpenDocument Presentation",
    "category": "Presentation",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".otp",
    "name": "OpenDocument Presentation Template",
    "category": "Presentation",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".pot",
    "name": "PowerPoint Template",
    "category": "Presentation",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".potm",
    "name": "PowerPoint Open XML Macro-Enabled Presentation Template",
    "category": "Presentation",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".potx",
    "name": "PowerPoint Open XML Presentation Template",
    "category": "Presentation",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".pps",
    "name": "PowerPoint Slide Show",
    "category": "Presentation",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".ppsm",
    "name": "PowerPoint Open XML Macro-Enabled Slide",
    "category": "Presentation",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".ppsx",
    "name": "PowerPoint Open XML Slide Show",
    "category": "Presentation",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".ppt",
    "name": "PowerPoint Presentation",
    "category": "Presentation",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".pptm",
    "name": "PowerPoint Open XML Macro-Enabled Presentation",
    "category": "Presentation",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".pptx",
    "name": "PowerPoint Open XML Presentation",
    "category": "Presentation",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".csv",
    "name": "Comma Separated Values File",
    "category": "Spreadsheet",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".numbers",
    "name": "Apple iWork Numbers",
    "category": "Spreadsheet",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".ods",
    "name": "OpenDocument Spreadsheet",
    "category": "Spreadsheet",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".ots",
    "name": "OpenDocument Spreadsheet Template",
    "category": "Spreadsheet",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".xla",
    "name": "Excel Add-In File",
    "category": "Spreadsheet",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".xlam",
    "name": "Excel Open XML Macro-Enabled Add-In",
    "category": "Spreadsheet",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".xls",
    "name": "Excel Spreadsheet",
    "category": "Spreadsheet",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".xlsb",
    "name": "Excel Binary Spreadsheet",
    "category": "Spreadsheet",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".xlsm",
    "name": "Excel Open XML Macro-Enabled Spreadsheet",
    "category": "Spreadsheet",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".xlsx",
    "name": "Microsoft Excel Open XML Spreadsheet",
    "category": "Spreadsheet",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".xlt",
    "name": "Microsoft Excel Template",
    "category": "Spreadsheet",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".xltm",
    "name": "Excel Open XML Macro-Enabled Spreadsheet Template",
    "category": "Spreadsheet",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".xltx",
    "name": "Excel Open XML Spreadsheet Template",
    "category": "Spreadsheet",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "✅",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ✅ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ❌"
  },
  {
    "ext": ".doc",
    "name": "Microsoft Word Document",
    "category": "WordProcessing",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "✅",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ✅ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".docm",
    "name": "Word Open XML Macro-Enabled Document",
    "category": "WordProcessing",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "✅",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ✅ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".docx",
    "name": "Microsoft Word Open XML Document",
    "category": "WordProcessing",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "✅",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ✅ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".dot",
    "name": "Word Document Template",
    "category": "WordProcessing",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "✅",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ✅ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".dotm",
    "name": "Word Open XML Macro-Enabled Document Template",
    "category": "WordProcessing",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "✅",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ✅ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".dotx",
    "name": "Word Open XML Document Template",
    "category": "WordProcessing",
    "support": {
      "ExtractContainersAndAttachments": "✅",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "✅",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ✅ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ✅ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".odt",
    "name": "OpenDocument Text Document",
    "category": "WordProcessing",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "✅",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ✅ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".ott",
    "name": "OpenDocument Document Template",
    "category": "WordProcessing",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "✅",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ✅ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".rtf",
    "name": "Rich Text Format File",
    "category": "WordProcessing",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "✅",
      "ExtractMetadata": "✅",
      "ExtractStructuredAndFormattedText": "✅",
      "ExtractTableOfContents": "✅",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "✅",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "✅",
      "ParseFormData": "❌",
      "ScanBarcode": "✅"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ✅ | ExtractMetadata: ✅ | ExtractStructuredAndFormattedText: ✅ | ExtractTableOfContents: ✅ | ExtractTextAccurate: ✅ | ExtractTextAreas: ✅ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ✅ | ParseFormData: ❌ | ScanBarcode: ✅"
  },
  {
    "ext": ".txt",
    "name": "Plain Text File",
    "category": "WordProcessing",
    "support": {
      "ExtractContainersAndAttachments": "❌",
      "ExtractImages": "❌",
      "ExtractMetadata": "❌",
      "ExtractStructuredAndFormattedText": "❌",
      "ExtractTableOfContents": "❌",
      "ExtractTextAccurate": "✅",
      "ExtractTextAreas": "❌",
      "ExtractTextRaw": "❌",
      "ParseDocumentByTemplate": "❌",
      "ParseFormData": "❌",
      "ScanBarcode": "❌"
    },
    "supportSummary": "ExtractContainersAndAttachments: ❌ | ExtractImages: ❌ | ExtractMetadata: ❌ | ExtractStructuredAndFormattedText: ❌ | ExtractTableOfContents: ❌ | ExtractTextAccurate: ✅ | ExtractTextAreas: ❌ | ExtractTextRaw: ❌ | ParseDocumentByTemplate: ❌ | ParseFormData: ❌ | ScanBarcode: ❌"
  }
];

    function performSearch(query) {
        if (!query || query.length < 2) {
            searchResults.style.display = 'none';
            return;
        }
        query = query.toLowerCase();
        const matches = formats.filter(format =>
            format.ext.toLowerCase().includes(query) ||
            format.name.toLowerCase().includes(query) ||
            format.category.toLowerCase().includes(query)
        );
        if (matches.length > 0) {
            resultsList.innerHTML = matches.map(match => `
                <div style="padding: 8px; margin: 5px 0; background: #f0f8ff; border-left: 3px solid #007bff; border-radius: 4px;">
                    <strong>${match.ext}</strong> - ${match.name}
                    <br><small>Category: ${match.category} | ${match.supportSummary}</small>
                </div>
            `).join('');
            searchResults.style.display = 'block';
        } else {
            resultsList.innerHTML = '<div style="padding: 8px; color: #666;">No formats found matching your search.</div>';
            searchResults.style.display = 'block';
        }
    }

    searchInput.addEventListener('input', function(e) {
        performSearch(e.target.value);
    });

    searchInput.addEventListener('keydown', function(e) {
        if (e.key === 'Escape') {
            searchResults.style.display = 'none';
            searchInput.value = '';
        }
    });

    document.addEventListener('click', function(e) {
        if (!e.target.closest('#formatSearch') && !e.target.closest('#searchResults')) {
            searchResults.style.display = 'none';
        }
    });
});
</script>
