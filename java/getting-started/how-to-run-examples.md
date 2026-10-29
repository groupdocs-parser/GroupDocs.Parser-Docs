---
id: how-to-run-examples
url: parser/java/how-to-run-examples
title: How to Run Examples
weight: 7
description: "Step-by-step guide to run GroupDocs.Parser for Java code examples. Learn how to build a project from scratch, run the GitHub examples with Maven or your IDE, and set up your development environment."
keywords: parser api, run examples, Java examples, GitHub examples, code samples, Maven
productName: GroupDocs.Parser for Java
hideChildren: False
toc: true
tags: java, parser, examples, setup, v26.10
---
{{< alert style="warning" >}}Before running an example make sure that GroupDocs.Parser has been installed successfully.{{< /alert >}}

We offer multiple solutions on how you can run GroupDocs.Parser examples, by building your own or using our examples out-of-the-box.

Please choose one from the following list:

## Build project from scratch

*   Open your IDE (for example, IntelliJ IDEA) and create a new **Maven** or **Gradle** project;
*   Install **GroupDocs.Parser for Java** from the GroupDocs Repository or official GroupDocs website following this [guide]({{< ref "parser/java/getting-started/installation.md" >}});
*   Code your first application with **GroupDocs.Parser for Java** like this:
    
    ```java
    // Create an instance of Parser class
    try (Parser parser = new Parser("c:\\documents\\sample.docx")) { // NOTE: Put here actual path for your document
        // Extract a text to the reader
        try (TextReader reader = parser.getText()) {
            // Print an extracted text (or "not supported" message)
            System.out.println(reader == null ? "Text extraction isn't supported" : reader.readToEnd());
        }
    }
    ```
    
*   Build and Run your project;
*   Extracted text will appear on the console.

## Run examples

The complete examples package of **GroupDocs.Parser** is hosted on [GitHub](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java). You can either download the ZIP file from [here](https://github.com/groupdocs-parser/GroupDocs.Parser-for-Java/archive/master.zip) or clone the repository of GitHub using your favorite git client.

In case you download the ZIP file, extract the folders on your local disk. The extracted files and folders will look like following image:

![](/parser/java/images/how-to-run-examples.png)

The **Examples** folder contains a Maven project (`Examples/pom.xml`) that references GroupDocs.Parser for Java from the GroupDocs Repository, so Maven downloads the library automatically. The **Examples/Resources/SampleFiles** folder contains all the sample documents used in the examples.

`RunExamples.java` is the entry point of the project. Every example is a class with a static `run` method, and `RunExamples.main` calls them one by one. Un-comment the example you want to run and comment the rest.

### Run from the command line

From the `Examples` directory run:

```bash
mvn compile exec:java -Dexec.mainClass=com.groupdocs.parser.examples.RunExamples
```

The sample documents are resolved relative to the working directory (see `Constants.SamplesPath`), so run the examples with the `Examples` directory as the working directory.

### Run from IntelliJ IDEA

Open IntelliJ IDEA and import the project to get started with it: select **File** -> **Open** and choose the `Examples/pom.xml` file, then open it as a project. Maven resolves the GroupDocs.Parser dependency automatically.

If you installed GroupDocs.Parser for Java from a downloaded JAR instead, add it to the module dependencies:

*   Click file from the toolbar
*   Open project structure using  (CTRL + SHIFT + ALT + S on Windows/Linux, ⌘ + ; on Mac OS X)
*   Select Modules at the left panel
*   Open Dependencies tab
*   Click '+' and click JARs or directories

![](/parser/java/images/how-to-run-examples_1.png)

Then run `RunExamples.main` with `Examples` as the working directory.

### Apply a license

Without a license the examples run in evaluation mode (see [Evaluation Limitations and Licensing]({{< ref "parser/java/getting-started/evaluation-limitations-and-licensing.md" >}})). If you have a license:

1.  Open `Constants.java` and set the `LicensePath` constant to your license file's path;
2.  Call `SetLicenseFromFile.run()` from `RunExamples.java` before any other example.

The examples of the `using_ocr` package additionally require an Aspose.OCR for Java license; set its path in the `Constants.AsposeOcrLicensePath` constant.

## Contribute

If you like to add or improve an example, we encourage you to contribute to the project. All examples in this repository are open source and can be freely used in your own applications.

To contribute, you can fork the repository, edit the code example and create a pull request. We will review the changes and include it in the repository if found helpful.
