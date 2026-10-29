---
id: system-requirements
url: parser/java/system-requirements
title: System Requirements
weight: 3
description: "System requirements for GroupDocs.Parser for Java including supported operating systems, Java versions, build tools and development environments."
keywords: parser, GroupDocs.Parser for Java, system requirements, Java 8, JDK, JRE, Maven, Gradle, prerequisites
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
tags: java, parser, system-requirements, prerequisites, v26.10
---
## Overview

GroupDocs.Parser for Java does not require any external software or third party tool to be installed. Just follow one of the way as described in [Development Environment, Installation and Configuration]({{< ref "parser/java/getting-started/installation.md" >}}).

An internet connection is not required to run GroupDocs.Parser or to validate a license file. Note, however, that documents referencing remote images or stylesheets can cause outbound requests during extraction, and a metered license reports usage online — see [Network access and data privacy]({{< ref "parser/java/getting-started/network-access-and-data-privacy.md" >}}).

## Supported Operating Systems

GroupDocs.Parser for Java supports any 32-bit or 64-bit operating system that runs the Java runtime including, but not limited to:

### Windows

*   Microsoft Windows 2003 Server ( x64, x86)
*   Microsoft Windows 2008 Server ( x64, x86)
*   Microsoft Windows 2012 Server ( x64, x86)
*   Microsoft Windows 2012 R2 Server ( x64, x86)
*   Microsoft Windows 2016 Server ( x64, x86)
*   Microsoft Windows 2019 Server ( x64, x86)
*   Microsoft Windows Vista ( x64, x86)
*   Microsoft Windows XP ( x64, x86)
*   Microsoft Windows 7 ( x64, x86)
*   Microsoft Windows 8, 8.1 ( x64, x86)
*   Microsoft Windows 10 ( x64, x86)
*   Microsoft Azure

### Linux

*   Linux (Ubuntu, OpenSUSE, CentOS, and others)

### Mac

*   Mac OS X

## Supported Java Versions

GroupDocs.Parser for Java is compiled for Java 8 and supports:

*   Java SE 8 (1.8)
*   or above

Any Java SE compatible JRE or JDK can be used, for example Oracle Java or OpenJDK builds.

{{< alert style="info" >}}
**Note:** [Metered licensing]({{< ref "parser/java/getting-started/evaluation-limitations-and-licensing.md" >}}) requires Java 8 update 101 (8u101) or later.
{{< /alert >}}

## Build Tools

GroupDocs.Parser for Java is distributed as a JAR file in the [GroupDocs Repository](https://releases.groupdocs.com/java/repo/com/groupdocs/groupdocs-parser/), so it can be used with any build tool that resolves dependencies from a Maven repository:

*   Apache Maven
*   Gradle

The JAR can also be added to the classpath of a project directly.

## Development Environments

GroupDocs.Parser for Java can be used to develop applications in any development environment that targets the Java platform, for example:

*   IntelliJ IDEA
*   Eclipse
*   Apache NetBeans
*   Visual Studio Code with Java extensions
