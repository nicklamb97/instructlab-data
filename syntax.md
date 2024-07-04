# Syntax

The following terminology distinctions are made when dealing with language items:

- A **command** is an operation that you execute from an OpenROAD Workbench menu or at the operating system level.
- A **statement** is an operation that you place within a program or called procedure. Statements can be written in OpenROAD 4GL, a database query language (such as SQL), or a 3GL (like C or COBOL).

When representing language elements in *discussion text*, the following conventions are used:

| Convention | Usage |
|------------|-------|
| UPPERCASE | Indicates constants (for example, TRUE, FALSE, and FB_DIMMED) |
| lowercase | Indicates 4GL keywords and statements (for example, begin keyword and callproc statement) |
| *italic, lowercase* | Indicates a variable name (for example, *cursor_variable*) |
| MixedCase/Initial Capitalization | Indicates class names, attributes, methods, or events (for example, DataStream class, ClientData attribute, FetchRow method, and WindowResized event) |

When representing *syntax*, the following conventions are used:

| Convention | Usage |
|------------|-------|
| **Boldface** | Indicates keywords, symbols, or punctuation that you must type as shown |
| *Italic* | Indicates a variable name for which you must supply an actual value—this convention is used in explanatory text and syntax |
| *Italic, underlined* | Indicates a variable name that can be used in a statement either dynamically (when you run the application) or statically (when you create the application) |
| [ ] (square brackets) | Indicates an optional item |
| { } (curly braces) | Indicates an optional item that you can repeat as many times as appropriate |
| \| (vertical bar) | Indicates a list of mutually exclusive items (that is, you can select only one item from the list) |

The following example illustrates the syntax conventions:

**select** [**all** \| **distinct**] *resultexpression*{**, ***resultexpression*} **from** *tablename* [*corrname*]{**, ***tablename* [*corrname*]} [**where** *searchcondition*] [**group by** *columnname*{**, ***columnname*}] [**having** *searchcondition*]
