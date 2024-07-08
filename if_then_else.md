# IF-THEN-ELSE Statement in Actian 4GL

The IF-THEN-ELSE statement is a fundamental control structure in Actian 4GL that allows for conditional execution of 
code blocks based on specified conditions. This structure is crucial for directing the flow of a program, enabling it 
to make decisions and execute specific code segments accordingly.

## Basic Syntax

The basic syntax of the IF-THEN-ELSE statement in Actian 4GL is as follows:

```4gl
IF condition THEN
    statements
ELSE IF condition THEN
    statements
ELSE
    statements
END IF
```

## Key Points

### Optional Clauses

- The ELSE IF and ELSE clauses are optional.
- An IF statement can be as simple as a single condition and a single block of statements to execute if that condition 
is true.
- Multiple ELSE IF clauses can be included to check additional conditions sequentially.

### Boolean Evaluation

- Conditions must evaluate to a boolean value (TRUE or FALSE).
- Conditions are typically composed of comparisons or logical expressions.

### Case-Insensitivity

- Actian 4GL is case-insensitive for keywords, so IF, if, and If are all valid, though conventionally, all caps should 
be used for readability and consistency.

## Condition Evaluation

Conditions in an IF-THEN-ELSE statement are evaluated in sequence from top to bottom. As soon as a true condition is 
found, the corresponding block of statements is executed, and the remaining conditions are ignored.

## Comparison Operators

Comparison operators are used to form conditions that compare values. The primary comparison operators in Actian 4GL 
include:

- `=` : Equal to
- `!=` or `<>` : Not equal to
- `<` : Less than
- `>` : Greater than
- `<=` : Less than or equal to
- `>=` : Greater than or equal to

## Logical Operators

Logical operators are used to combine multiple conditions or invert a condition:

- `AND` : Logical AND
- `OR` : Logical OR
- `NOT` : Logical NOT
