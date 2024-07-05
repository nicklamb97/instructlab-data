# Literals

A literal is an explicit representation of a value. There are three types of literals:
- String
- Numeric
- Date/time

## Unicode Literals

To specify a Unicode literal value within a non-Unicode command string (for example, in a query entered into the terminal monitor), the Unicode literal notation can be used. A Unicode literal is a sequence of ASCII characters intermixed with escaped sequence of hex digits, all enclosed in quotes and preceded by U&. The escape character \ precedes sets of 4 hex digits that are treated as Unicode codepoints from Plane 0 (the Basic Multilingual Plane) and the escape sequence \+ precedes sequences of 6 hex digits that are treated as codepoints from the Supplementary Plane. Note that this may require a leading 0. For example:

```
U&'Hello\202Fworld\+029E71'
```

In this string, Hello is converted to the equivalent Unicode codepoints, the hex digits 202F represent codepoint U+202F (the narrow non-breaking space), world is converted to the equivalent Unicode codepoints, and the hex digits 029E71 represent codepoint U+29E71 (the Chinese ideograph chù from Plane 2). The resulting literal is the concatenation of the converted Unicode components.

The notation N'x' can be used as an alternative to the U&'x' notation.

## String Literals

String literals are specified by one or more characters enclosed in single quotes. The default data type for string literals is varchar, but a string literal can be assigned to any character data type or to money or date data type without using a data type conversion function.

To compare a string literal with a non-character data type (A), you must either cast the string literal to the non-character data type A, or cast the non-character data type to the string literal type. Failure to do so causes unexpected results if the non-character data type contains the 'NULL (0) value.

For example, to compare the function X string literal that returns a varchar data type to a byte data type, cast the result of the X function as follows:

```sql
SELECT * FROM uid_table
WHERE HEX(uid) = '010000000000000000000000000000'
```

### Hexadecimal Representation

To specify a non-printing character in terminal monitor, use a hex (hexadecimal) constant. Hex constants are specified by an X followed by a single-quoted string composed of (an even number of) alphanumeric characters. For example, the following represents the ASCII string ABC<carriage return>:

```
X'4142430D'
```

A = X'41', B = X'42', C = X'43', and carriage return = X'OD'.

### Quotes within Strings

To include a single quote inside a string literal, it must be doubled. For example:

```
'The following letter is quoted: ''A''.'
```

which is evaluated as:

```
The following letter is quoted: 'A'.
```

## Numeric Literals

Numeric literals specify numeric values. There are three types of numeric literals:
- Integer
- Decimal
- Floating point

A numeric literal can be assigned to any of the numeric data types or the money data type without using an explicit conversion function. The literal is automatically converted to the appropriate data type, if necessary.

By default, the period (.) is displayed to indicate the decimal point. This default can be changed by setting II_DECIMAL. For information about setting II_DECIMAL, see the Workbench User Guide.

Note: If II_DECIMAL is set to comma, be sure that when SQL syntax requires a comma (such as a list of table columns or SQL functions with several parameters), that the comma is followed by a space. For example:

```sql
select col1, ifnull(col2, 0), left(col4, 22) from t1;
```

### Integer Literals

Integer literals are specified by a sequence of 10 digits and an optional sign, in the following format:

```
[+|-] digit {digit} [e digit]
```

Integer literals are represented internally as an integer, smallint, or bigint depending on the value of the literal. If the literal is within the range -32,768 to +32,767, it is represented as a smallint. If its value is within the range -2,147,483,648 to +2,147,483,647 but outside the range of a smallint, it is represented as an integer. A literal in the range -9,223,372,036,854,775,808 to +9,223,372,036,854,775,807 is represented as a bigint. Values that exceed the range of integers are represented as decimals.

You can specify integers using a simplified scientific notation, similar to the way floating point values are specified. To specify an exponent, follow the integer value with the letter, e, and the value of the exponent. This notation is useful for specifying large values. For example, to specify 100,000 use the exponential notation as follows:

```
1e5
```

### Decimal Literals

Decimal literals are specified as signed or unsigned numbers of 1 to 39 digits that include a decimal point. The precision of a decimal number is the total number of digits, including leading and trailing zeros. The scale of a decimal literal is the total number of digits to the right of the decimal point, including trailing zeros.

Decimal literals that exceed 39 digits are treated as floating point values.

Examples of decimal literals are:

```
3.
-10.
1234567890.12345
001.100
```

### Floating Point Literals

A floating point literal must be specified using scientific notation. The format is:

```
[+|-] {digit} [.{digit}] e|E [+|-] {digit}
```

For example:

```
2.3e-02
```

At least one digit must be specified, either before or after the decimal point.

## Named Constants

A named constant is a literal value to which you give a name. You can then use the name in place of the constant in any 4GL expression. You specify the value for the named constant when you create it with the OpenROAD Workbench. This constant is global to the application.

You must use the Constant Editor to change the value of a constant. Although you can reference the named constant in your 4GL code, you cannot change its value at runtime.

Constants let you substitute a brief name for a long phrase or value that is used in several places, for example, if you have encoded a set of values as integers for storage. Constants also provide a consistent reference for message strings, for example, in internationalizing products.

To reference a named constant from your 4GL code, use the constant name, for example:

```
if salary > HISALARYVALUE then ...
```

For more information about using the Constant Editor, see the Workbench User Guide.

## System Variables

System variables are built-in variables that are available in all frames and scripts. OpenROAD provides the following system variables:

- CurEventScope
- CurScriptScope
- CurExec
- CurFrame
- CurMethod
- CurObject
- CurProcedure
- CurSession
- IIDBMSerror
- IIerrornumber
- IIrowcount

(Detailed descriptions of each system variable have been omitted for brevity. They can be added if needed.)
