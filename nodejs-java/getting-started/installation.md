---
id: installation
url: parser/nodejs-java/installation
title: Installation
weight: 4
description: "Install GroupDocs.Parser for Node.js via Java from npm, set up Java, verify the installation and troubleshoot common problems."
keywords: installation, npm, node.js, java, groupdocs parser
productName: GroupDocs.Parser for Node.js via Java
hideChildren: False
toc: True
---

## Prerequisites

- Node.js 20 LTS or later
- JDK 8 or later (17 LTS recommended) with `JAVA_HOME` set
- Native build tools for node-gyp

See [System Requirements](/parser/nodejs-java/system-requirements/) for details.

## Install from npm

GroupDocs.Parser for Node.js via Java is published on npm as [@groupdocs/groupdocs.parser](https://www.npmjs.com/package/@groupdocs/groupdocs.parser). Install it into your project:

```bash
npm install @groupdocs/groupdocs.parser
```

During installation npm:

1. Builds the [java](https://www.npmjs.com/package/java) bridge package, which lets Node.js call the Java API.
2. Runs the package's `postinstall` script, which downloads the GroupDocs.Parser JAR file (`groupdocs-parser-nodejs-<version>.jar`) into `node_modules/@groupdocs/groupdocs.parser/lib/`.

## Verify installation

Create a file named `check.js`:

```javascript
'use strict';

const groupdocs = require('@groupdocs/groupdocs.parser');

const formats = groupdocs.FileType.getSupportedFileTypes();
console.log('GroupDocs.Parser loaded, supported file types:', formats.size());

process.exit(0);
```

Run it:

```bash
node check.js
```

{{< alert style="info" >}}
GroupDocs.Parser runs inside a Java virtual machine started by the `java` bridge, and the JVM keeps the Node.js process alive after your code finishes. Call `process.exit()` at the end of standalone scripts.
{{< /alert >}}

## Download the JAR file manually

If the `postinstall` step cannot download the JAR file (for example, behind a corporate proxy), the package prints a warning when you load it. Download the file again from the package folder:

```bash
cd node_modules/@groupdocs/groupdocs.parser
npm run postinstall
```

Or download `groupdocs-parser-nodejs-<version>.jar` from `https://releases.groupdocs.com/java/repo/com/groupdocs/groupdocs-parser-nodejs/<version>/` and place it in `node_modules/@groupdocs/groupdocs.parser/lib/`.

## Troubleshooting

- **node-gyp build errors**: install the build tools listed in [System Requirements](/parser/nodejs-java/system-requirements/#build-tools), restart the terminal and run `npm install` again.
- **`jni.h` not found**: a JRE is installed instead of a JDK, or `JAVA_HOME` points to the wrong folder.
- **Java not found at runtime**: set `JAVA_HOME` and add `JAVA_HOME/bin` to `PATH`.
- **The script does not exit**: add `process.exit()` at the end of the script.
- **Permission errors when saving images or previews**: make sure the process can write to the output folder.
