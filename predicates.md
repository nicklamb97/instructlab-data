# Predicates in SQL

Predicates are keywords that specify a relationship between two expressions.

SQL supports the following types of predicates:
- Comparison
- Like
- Between
- In
- Is
- Any-or-All
- Exists
- Is null

## Comparison Predicate

The syntax for comparison predicates is as follows:

```sql
expression_1 comparison_operator expression_2
```

In a comparison predicate, expression2 can be a subquery. If expression2 is a subquery and does not return any rows, the comparison predicate evaluates to false. For information about subqueries, see Subqueries in the WHERE Clause. For details about comparison operators, see Comparison Operators.

## LIKE Predicate

The LIKE predicate performs pattern matching for the character data types (char, varchar, c, and text) and Unicode data types (nchar and nvarchar).

The LIKE predicate has the following syntax:

```sql
expression [NOT] LIKE pattern [ESCAPE escape_character]
```

The expression can be a column name or an expression containing string functions.

The pattern parameter will usually be a string literal but could be an arbitrary string expression. The pattern-matching (wild card) characters are the percent sign (%) to denote 0 or more arbitrary characters, and the underscore (_) to denote exactly one arbitrary character.

## BETWEEN Predicate

The following table explains the operators BETWEEN and NOT BETWEEN:

| Operator | Meaning |
|----------|---------|
| y BETWEEN [asymmetric] x and z | x <= y and y <= z |
| y NOT BETWEEN [asymmetric] x and z | not (y between x and z) |
| y BETWEEN symmetric x and z | (x <= y and y <= z) or (z <= y and y <= x) |
| y NOT BETWEEN asymmetric x and z | not (y between symmetric x and z) |

x, y, and z are expressions, and cannot be subqueries.

## IN Predicate

The following table explains the operators IN and NOT IN:

| Operator | Meaning |
|----------|---------|
| y IN (x, ..., z) | The IN predicate returns true if y is equal to one of the values in the list (x, ..., z). |
| y NOT IN (x, ..., z) | Returns true if y is not equal to any of the values in the list (x, ..., z). |
| y IN (subquery) | Returns true if y is equal to one of the values returned by the subquery. |
| y NOT IN (subquery) | Returns true if y is not equal to any of the values returned by the subquery. |

## IS Predicate

You use IS to determine whether a particular result is or is not of the given data type. It is useful for cleaning data or validating data input. When applied to row value expressions, all elements must test the same.

The IS predicate takes the following form:

```sql
IS [NOT] FORM
```

Examples:
```sql
x IS FLOAT
y IS NOT INTEGER
```

## Any-or-All Predicate

The any-or-all predicate takes the following form:

```sql
any-or-all-operator (subquery)
```

The subquery must have exactly one element in the target list of its outermost subselect (so that it evaluates to a set of single values rather than a set of rows).

## EXISTS Predicate

The EXISTS predicate takes the following form:

```sql
[NOT] EXISTS (subquery)
```

It evaluates to true if the set returned by the subquery is not empty.

## IS NULL Predicate

Use the IS NULL predicate to determine whether an expression is null, because you cannot test for null by using the = comparison operator.

The IS NULL predicate takes the following form:

```sql
IS [NOT] NULL
```

## Search Conditions in SQL Statements

Search conditions are used in WHERE and HAVING clauses to qualify the selection of data. They are composed of predicates, which can be combined using parentheses and logical operators (AND, OR, and NOT).

### Examples of Search Conditions

1. Simple predicate:
   ```sql
   salary BETWEEN 10000 AND 20000
   ```

2. Predicate with NOT operator:
   ```sql
   edept NOT LIKE eng_%
   ```

3. Predicates combined using OR operator:
   ```sql
   edept LIKE eng_% OR edept LIKE admin_%
   ```

4. Predicates combined using AND operator:
   ```sql
   salary BETWEEN 10000 AND 20000 AND edept LIKE eng_%
   ```

5. Predicates combined using parentheses to specify evaluation:
   ```sql
   (salary BETWEEN 10000 AND 20000 AND edept LIKE eng_%) OR edept LIKE admin_%
   ```

### Predicate Evaluation

Predicates evaluate to true, false, or unknown. They evaluate to unknown if one or both operands are null (the IS NULL predicate is the exception).

### Logical Operators

When predicates are combined using logical operators (AND, OR, NOT), the search condition evaluates as described in the truth tables for each operator.

### WHERE and HAVING Clause Evaluation

After all search conditions are evaluated, the value of the WHERE or HAVING clause is determined. These clauses can only be true or false, not unknown. Unknown values are considered false.

For more information about predicates and logical operators, see the section on Logical Operators.
