---
id: logging
url: parser/python-net/logging
title: Logging
weight: 8
description: "Receive the errors, warnings and events GroupDocs.Parser for Python via .NET reports while it extracts data, with a Python function, an object, or Python's logging module."
keywords: logging, logger, ParserSettings, trace, warning, error, python logging
productName: GroupDocs.Parser for Python via .NET
hideChildren: False
toc: true
---

GroupDocs.Parser can report the errors, warnings and events that occur while it extracts data. Pass a logger to `ParserSettings` and the settings to the `Parser` constructor. The logger can be:

| Logger | Called as |
| --- | --- |
| A function | `log(level, message, exception)`, where `level` is `"trace"`, `"warning"` or `"error"`, and `exception` is the text of the .NET exception or `None` |
| An object | `trace(message)`, `warning(message)` and `error(message, exception)`; a method the object does not define is skipped |

## Log with a function

Here are the steps to receive the information with a function:

1. Define a function that takes `level`, `message` and `exception`;
2. Create `ParserSettings` with the function;
3. Instantiate the `Parser` object with the settings.

{{< tabs "example-1" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.exceptions import InvalidPasswordException
from groupdocs.parser.options import ParserSettings


def log(level, message, exception):
    # level is "trace", "warning" or "error";
    # exception is the text of the .NET exception, or None
    print("%s: %s" % (level.capitalize(), message))


try:
    # Open a protected document without its password
    with Parser("./protected.pdf", ParserSettings(log)) as parser:
        print(parser.get_text())
except InvalidPasswordException:
    print("The document is password-protected.")
```

{{< /tab >}}
{{< tab "protected.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [protected.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/logging/protected.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "log.txt" >}}  
```text
ERROR groupdocs.parser: Exception of type 'GroupDocs.Parser.Exceptions.InvalidPasswordException' was thrown.
INFO groupdocs.parser: Skipped a protected document
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/logging/log/log.txt)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** The logger reports the error before the exception reaches your code:

```text
Error: Exception of type 'GroupDocs.Parser.Exceptions.InvalidPasswordException' was thrown.
The document is password-protected.
```

## Log with an object

An object with `trace`, `warning` and `error` methods works too, the shape of the .NET `ILogger` interface:

{{< tabs "example-2" >}}
{{< tab "Python" >}}

```python
from groupdocs.parser import Parser
from groupdocs.parser.exceptions import InvalidPasswordException
from groupdocs.parser.options import ParserSettings


class Logger:
    def trace(self, message):
        # Print event message
        print("Event: " + message)

    def warning(self, message):
        # Print warning message
        print("Warning: " + message)

    def error(self, message, exception):
        # Print error message
        print("Error: " + message)


try:
    with Parser("./protected.pdf", ParserSettings(Logger())) as parser:
        print(parser.get_text())
except InvalidPasswordException:
    pass  # The logger has already reported it
```

{{< /tab >}}
{{< tab "protected.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [protected.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/logging/protected.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "error.txt" >}}  
```text
Error: Exception of type 'GroupDocs.Parser.Exceptions.InvalidPasswordException' was thrown.
```
[Download full output](/parser/python-net/_output_files/developer-guide/advanced-usage/logging/error/error.txt)
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:** Prints `Error: Exception of type 'GroupDocs.Parser.Exceptions.InvalidPasswordException' was thrown.`

## Forward to Python's logging module

Map the three levels onto the standard `logging` levels to route the engine's messages to your application's handlers:

{{< tabs "example-3" >}}
{{< tab "Python" >}}

```python
import logging
import sys

from groupdocs.parser import Parser
from groupdocs.parser.exceptions import InvalidPasswordException
from groupdocs.parser.options import ParserSettings

logging.basicConfig(stream=sys.stdout, level=logging.DEBUG,
                    format="%(levelname)s %(name)s: %(message)s")
logger = logging.getLogger("groupdocs.parser")
LEVELS = {"trace": logging.DEBUG, "warning": logging.WARNING, "error": logging.ERROR}


def log(level, message, exception):
    logger.log(LEVELS.get(level, logging.INFO), message)


try:
    with Parser("./protected.pdf", ParserSettings(log)) as parser:
        parser.get_text()
except InvalidPasswordException:
    logger.info("Skipped a protected document")
```

{{< /tab >}}
{{< tab "protected.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [protected.pdf](/parser/python-net/_sample_files/developer-guide/advanced-usage/logging/protected.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

**Expected behavior:**

```text
ERROR groupdocs.parser: Exception of type 'GroupDocs.Parser.Exceptions.InvalidPasswordException' was thrown.
INFO groupdocs.parser: Skipped a protected document
```

## Notes

- `ParserSettings` has no empty constructor: pass a logger, an OCR connector or an external resource handler. To pass a logger together with a handler, pass all three in order, with `None` for the OCR connector: `ParserSettings(log, None, on_loading)`.
- The engine reports what it encounters; a document that opens and parses cleanly may produce no messages at all.

## Related pages

- [Handle loading of external resources]({{< ref "parser/python-net/developer-guide/advanced-usage/loading/handle-loading-of-external-resources" >}})
- [Errors and exceptions]({{< ref "parser/python-net/developer-guide/advanced-usage/errors-and-exceptions" >}})
- [Load password-protected documents]({{< ref "parser/python-net/developer-guide/advanced-usage/loading/load-password-protected-documents" >}})
