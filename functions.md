# SQL Functions in OpenROAD

SQL functions in OpenROAD can be used in SELECT, INSERT, UPDATE, DELETE, WHILE, and IF statements. There are two main types:

1. Scalar functions - Take single-valued expressions as arguments
2. Aggregate functions - Take a set of values as arguments

## Scalar Functions

Major categories include:

- Data Type Conversion Functions (e.g. CAST, TO_CHAR)
- Numeric Functions (e.g. ABS, ROUND, SQRT)  
- String Functions (e.g. CONCAT, SUBSTRING, UPPER)
- Date Functions (e.g. YEAR, MONTH, DAY)
- HASH Functions
- Random Number Functions

Some key scalar functions:

```sql
IFNULL(expr1, expr2) -- Returns expr2 if expr1 is null
COALESCE(expr1, expr2, ...) -- Returns first non-null expression
CAST(expr AS type) -- Converts expr to specified data type
SUBSTRING(string, start, length) -- Extracts substring
UPPER(string) -- Converts string to uppercase
LOWER(string) -- Converts string to lowercase
ROUND(number, decimals) -- Rounds number to specified decimals
CURRENT_DATE -- Returns current date
```

## Aggregate Functions 

Major categories:

- Unary (e.g. SUM, AVG, COUNT)
- Binary (e.g. CORR, COVAR_POP)

Some key aggregate functions:

```sql
SUM(expr) -- Sum of values
AVG(expr) -- Average of values  
COUNT(*) -- Count of rows
MAX(expr) -- Maximum value
MIN(expr) -- Minimum value
```

Aggregate functions are often used with GROUP BY clauses.

## Key Points

- Functions can be nested 
- Aggregate functions cannot be used in WHERE clauses
- DISTINCT keyword can be used with aggregates to eliminate duplicates
- NULL values are generally ignored by aggregate functions (except COUNT(*))
- The IFNULL function provides a way to handle null values
