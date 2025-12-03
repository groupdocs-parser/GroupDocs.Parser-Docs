---
id: installation
url: parser/net/installation
title: Installation
weight: 4
version: 23.5
description: "Installation guide for GroupDocs.Parser for .NET. Learn how to install via NuGet, configure for .NET Core/.NET 5+, and set up prerequisites for different frameworks."
keywords: install GroupDocs.Parser, NuGet package, .NET Core installation, .NET Framework setup
productName: GroupDocs.Parser for .NET
hideChildren: False
toc: true
tags: csharp, parser, installation, setup, v23.5
---
## Prerequisites

### Framework Requirements

GroupDocs.Parser for .NET supports the following frameworks:

| Framework | Minimum Version | Package |
|-----------|----------------|---------|
| .NET Framework | 4.6.1 | `GroupDocs.Parser.Framework` |
| .NET Core | 2.0 | `GroupDocs.Parser` |
| .NET Standard | 2.0 | `GroupDocs.Parser` |
| .NET | 5.0+ | `GroupDocs.Parser` |

{{< alert style="info" >}}
**For .NET Framework 4.5 and earlier:** You need to upgrade to .NET Framework 4.6.1 or higher, or target .NET Standard 2.0 compatible packages. GroupDocs.Parser requires .NET Standard 2.0 compatibility.
{{< /alert >}}

## Install from NuGet

NuGet is the easiest way to download and install GroupDocs.Parser for .NET. There are multiple ways to install it in your project.

#### Install via Package Manager GUI

Follow these steps to reference GroupDocs.Parser using Package Manager GUI:

*   Open your solution/project in Visual Studio.
*   Click Tools -> NuGet Package Manager -> Manage NuGet Packages for Solution.  
    You can also access the same option through the Solution Explorer. Right-click the solution or project and select Manage NuGet Packages from the context menu
*   Select Browse tab and type "GroupDocs.Parser" in the search text box.
*   Click the Install button to install the latest version of the API into your project as shown in the following screenshot.

![](/parser/net/images/installation.png)

#### Using Package Manager Console

You can follow the steps below to reference GroupDocs.Parser for .NET using the Package Manager Console:

*   Open your solution/project in Visual Studio.
*   Select Tools -> NuGet Package Manager -> Package Manager Console from the menu to open package manager console.
*   Type the command based on your target framework:
    - For .NET Core/.NET 5+: `Install-Package GroupDocs.Parser`
    - For .NET Framework: `Install-Package GroupDocs.Parser.Framework`
*   Press enter to install the latest release into your application.
*   After successful installation, GroupDocs.Parser will be referenced in your application.

#### Using .NET CLI (.NET Core/.NET 5+)

For .NET Core, .NET 5, .NET 6, or later projects:

```bash
# Create a new console project
dotnet new console -n MyParserApp
cd MyParserApp

# Add the package
dotnet add package GroupDocs.Parser

# Restore packages
dotnet restore
```

#### Quick Start with .NET Core

Here's a complete example for setting up a .NET Core project:

```bash
# 1. Create new project
dotnet new console -n GroupDocsParserDemo
cd GroupDocsParserDemo

# 2. Install package
dotnet add package GroupDocs.Parser

# 3. Run the project
dotnet run
```

Then update `Program.cs`:

```csharp
using GroupDocs.Parser;

namespace GroupDocsParserDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            using (Parser parser = new Parser("sample.pdf"))
            {
                using (var reader = parser.GetText())
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

![](/parser/net/images/installation_1.png)

![](/parser/net/images/installation_2.png)

Starting with 24.2 version, GroupDocs.Parser is divided into two packages - for .NET Framework and .NET Standard. GroupDocs.Parser for .NET Framework package is recommended for using with .NET Framework projects. GroupDocs.Parser package is designed to work with .NET projects. You need to [configure binding redirection](https://stackoverflow.com/questions/43365736/assembly-binding-redirect-how-and-why) to make it work with .NET Framework. For regular .NET Framework project it's required to [enable automatic binding redirection](https://learn.microsoft.com/en-us/dotnet/framework/configure-apps/how-to-enable-and-disable-automatic-binding-redirection). For unit tests projects you also need to specify `GenerateBindingRedirectsOutputType` in the project file:

```
<PropertyGroup>
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
</PropertyGroup>
```

## Install from official GroupDocs website

You can follow the steps below to reference GroupDocs.Parser for .NET downloaded from official website Downloads section:

*   Unpack zip archive or follow MSI install wizard instructions.
*   In the Solution Explorer, expand the project node you want to add a reference to.
*   Right-click the References node for the project and select Add Reference from the menu.
*   In the Add Reference dialog box, select the .NET tab (it's usually selected by default).
*   If you have used MSI installer to install GroupDocs.Parser, you will see GroupDocs.Parser in the top pane. Select it and then click the Select button.
*   If you have downloaded and unpacked the DLL only, click the Browse button and locate the GroupDocs.Parser.dll file.
*   You have referenced GroupDocs.Parser and it should appear in the SelectedComponents pane of the dialog box.
*   Click OK.

GroupDocs.Parser reference appears under the References node of the project.

Note that .NET Standard 2.0 version has external references:

| Package | Version |
| --- | --- |
| System.Drawing.Common                  | 4.7.0  |
| System.Text.Encoding.CodePages         | 4.7.0  |
| System.Reflection.Emit                 | 4.7.0  |
| System.Reflection.Emit.ILGeneration    | 4.7.0  |
| System.Diagnostics.PerformanceCounter  | 4.5.0  |
| System.Security.Cryptography.Pkcs      | 4.7.0  |
| System.Security.Permissions            | 4.5.0  |
| Microsoft.Win32.Registry               | 4.7.0  |
| Microsoft.Extensions.DependencyModel   | 2.0.4  |
| SkiaSharp                              | 2.80.1 |

## Linux environment

For correct working of GroupDocs.Parser.Net on Linux the following packages need to be installed:

* libgdiplus package
* libc6-dev package
* package with Microsoft compatible fonts: ttf-mscorefonts-installer. (e.g. sudo apt-get install ttf-mscorefonts-installer)

Also GroupDocs.Parser.Net in Linux environment has external reference:

| Package | Version |
| --- | --- |
| SkiaSharp.NativeAssets.Linux.NoDependencies | 2.80.2 |