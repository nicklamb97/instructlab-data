## Operators in OpenROAD

OpenROAD supports various types of operators for different operations. Here's an overview of the supported operators:

### Arithmetic Operators

Arithmetic operators combine numeric expressions into new expressions. They are:

- `+`: Addition
- `-`: Subtraction
- `*`: Multiplication
- `/`: Division
- `**`: Exponentiation

Examples:

```
num_days = num_days + 30;
area = pi * r**2;
```

OpenROAD also supports date arithmetic:

```
start_date = start_date + '2 days';
```

#### Operator Precedence (highest to lowest):

1. `**`
2. `*`, `/`
3. `+`, `-`

Operators with equal precedence are processed from left to right. Use parentheses to force alternate precedence.

### String Operator

The concatenation operator (`+`) joins string expressions:

```
answer = prompt 'Please enter department for ' + name + ': ';
```

### Logical Operators

Logical operators (AND, OR, NOT) join logical expressions into new boolean expressions.

#### Truth Tables:

**AND:**
- True AND True = True
- True AND False = False
- True AND Null = Null
- False AND False = False
- False AND Null = False
- Null AND Null = Null

**OR:**
- True OR True/False/Null = True
- False OR True = True
- False OR False = False
- False OR Null = Null
- Null OR Null = Null

**NOT:**
- NOT True = False
- NOT False = True
- NOT Null = Null

#### Precedence (highest to lowest):

1. NOT
2. AND
3. OR

Use parentheses to change the order of evaluation.

### Comparison (Boolean) Operators

These operators yield boolean values (TRUE, FALSE, or null with nullable expressions):

- `=`: Equal to
- `!=`, `<>`, `^=`: Not equal to
- `<`: Less than
- `<=`: Less than or equal to
- `>`: Greater than
- `>=`: Greater than or equal to
- `is null`: Value is null
- `is not null`: Value is other than null
- `like`: Value matches a pattern-matching string
- `not like`: Value doesn't match a pattern-matching string

### Like Operator and Pattern Matching

The `like` operator compares two strings for resemblance. Syntax:

```
charvar [not] like pattern [escape escapechar]
```

Special characters in the pattern:
- `_`: Matches any single character
- `%`: Matches any string of characters, regardless of length
- `[]`: Matches any character within the brackets (when preceded by escape character)

### Is [Not] Null Operator

Tests whether an expression is null. Syntax:

```
expression is [not] null
```

Example:

```
if salary is null then
    sal_msg = 'Salary amount is unknown.'
endif;
```
