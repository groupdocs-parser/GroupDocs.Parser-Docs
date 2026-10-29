---
id: installation
url: parser/java/installation
title: Installation
weight: 4
description: "Installation guide for GroupDocs.Parser for Java. Learn how to add the library to Maven or Gradle projects from the GroupDocs repository, install it from a downloaded package, and prepare a Linux environment."
keywords: install GroupDocs.Parser for Java, Maven dependency, Gradle dependency, GroupDocs repository, Java document parser setup
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
tags: java, parser, installation, setup, v26.10
---
## Prerequisites

### Java requirements

GroupDocs.Parser for Java requires **Java 8 (1.8) or later** (JDK to build your application, JRE or JDK to run it). See [System Requirements]({{< ref "parser/java/getting-started/system-requirements.md" >}}) for the full list of supported operating systems and development environments.

| Item | Value |
|------|-------|
| Group ID | `com.groupdocs` |
| Artifact ID | `groupdocs-parser` |
| Current version | `26.10` |
| Repository | `https://releases.groupdocs.com/java/repo/` |

{{< alert style="info" >}}
The `groupdocs-parser` JAR is self-contained: its POM declares no transitive dependencies, so you add only the one artifact. A JDBC driver is needed only if you [extract data from databases]({{< ref "parser/java/developer-guide/advanced-usage/extract-data-from-databases.md" >}}), and an OCR library only if you plug in an [OCR connector]({{< ref "parser/java/developer-guide/advanced-usage/using-ocr/_index.md" >}}).
{{< /alert >}}

## Install from GroupDocs Repository using Maven

GroupDocs hosts all Java APIs on [GroupDocs Repository](https://releases.groupdocs.com/java/repo/). You can easily use [GroupDocs.Parser for Java](https://releases.groupdocs.com/java/repo/com/groupdocs/groupdocs-parser/) API directly in your Maven projects with simple configurations.

### Specify GroupDocs Repository Configuration

First, you need to specify GroupDocs repository configuration/location in your Maven `pom.xml` as follows:

```xml
<repositories>
    <repository>
        <id>GroupDocsJavaAPI</id>
        <name>GroupDocs Java API</name>
        <url>https://releases.groupdocs.com/java/repo/</url>
    </repository>
</repositories>
```

### Define GroupDocs.Parser for Java API Dependency

Then define GroupDocs.Parser for Java API dependency in your `pom.xml` as follows:

```xml
<dependencies>
    <dependency>
        <groupId>com.groupdocs</groupId>
        <artifactId>groupdocs-parser</artifactId>
        <version>26.10</version>
    </dependency>
</dependencies>
```

After performing above-mentioned steps, GroupDocs.Parser for Java dependency will finally be added to your Maven project. Run `mvn compile` (or reload the Maven project in your IDE) to download it.

## Install using Gradle

Add the GroupDocs repository and the dependency to your `build.gradle`:

```groovy
repositories {
    mavenCentral()
    maven {
        url "https://releases.groupdocs.com/java/repo/"
    }
}

dependencies {
    implementation "com.groupdocs:groupdocs-parser:26.10"
}
```

For the Kotlin DSL (`build.gradle.kts`):

```kotlin
repositories {
    mavenCentral()
    maven("https://releases.groupdocs.com/java/repo/")
}

dependencies {
    implementation("com.groupdocs:groupdocs-parser:26.10")
}
```

## Quick start with Maven

Here's a complete example of setting up a new console project:

```bash
# 1. Create a new project
mvn archetype:generate -DgroupId=com.example -DartifactId=parser-demo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
cd parser-demo

# 2. Add the GroupDocs repository and the groupdocs-parser dependency to pom.xml (see above),
#    and set the compiler level to Java 8 or later

# 3. Build and run the project
mvn compile exec:java -Dexec.mainClass=com.example.App
```

Then update `src/main/java/com/example/App.java`:

```java
package com.example;

import com.groupdocs.parser.Parser;
import com.groupdocs.parser.data.TextReader;

import java.io.IOException;

public class App {
    public static void main(String[] args) throws IOException {
        // Create an instance of Parser class
        try (Parser parser = new Parser("sample.pdf")) {
            // Extract a text to the reader
            try (TextReader reader = parser.getText()) {
                // Print an extracted text (or "not supported" message)
                System.out.println(reader == null ? "Text extraction isn't supported" : reader.readToEnd());
            }
        }
    }
}
```

## Install from official GroupDocs website

You can follow the steps below to reference GroupDocs.Parser for Java downloaded from official website [Downloads section](https://releases.groupdocs.com/parser/java/):

1.  Unpack zip archive.
2.  Switch to **lib** folder.
3.  Run **install.bat** (for Windows) or **install.sh** (for Linux) file to install the library in your local maven repository.
4.  After this you should add the GroupDocs.Parser for Java **dependency** block to your `pom.xml` project file.

If you don't use Maven or Gradle, add the `groupdocs-parser-26.10.jar` file to the classpath of your application (for example, in IntelliJ IDEA: **File** -> **Project Structure** -> **Modules** -> **Dependencies** -> **+** -> **JARs or directories**).

## Linux environment

GroupDocs.Parser for Java is pure Java: unlike the .NET version, it doesn't need native packages such as `libgdiplus` on Linux.

Text layout operations — page previews, text areas, formatted text and table detection in Word processing documents and presentations — use the fonts installed on the machine. Minimal Linux images often contain no fonts at all, which can change the measured positions of text. Install a set of TrueType fonts, for example the Microsoft compatible fonts:

```bash
sudo apt-get install ttf-mscorefonts-installer
```

## More resources

*   [System Requirements]({{< ref "parser/java/getting-started/system-requirements.md" >}})
*   [How to Run Examples]({{< ref "parser/java/getting-started/how-to-run-examples.md" >}})
*   [Evaluation Limitations and Licensing]({{< ref "parser/java/getting-started/evaluation-limitations-and-licensing.md" >}})
*   [GroupDocs.Parser for Java Maven Repository](https://releases.groupdocs.com/java/repo/com/groupdocs/groupdocs-parser/)
*   [GroupDocs.Parser for Java examples on GitHub](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java)
