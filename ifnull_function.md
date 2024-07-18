# IFNULL Function

The IFNULL function specifies a value other than a null that is returned to your application when a null is encountered. The ifnull function is specified as follows:

```
IFNULL(v1,v2)
```

If the value of the first argument is not null, IFNULL returns the value of the first argument. If the first argument evaluates to a null, IFNULL returns the second argument.

For example, the SUM, AVG, MAX, and MIN aggregate functions return a null if the argument to the function evaluates to an empty set. To receive a value instead of a null when the function evaluates to an empty set, use the IFNULL function, as in this example:

```
IFNULL(SUM(employee.salary)/25, -1)
```

IFNULL returns the value of the expression sum(employee.salary)/25 unless that expression is null. If the expression is null, the IFNULL function returns -1.

**Note:** If an attempt is made to use the IFNULL function with data types that are not nullable, such as system_maintained logical keys, a runtime error is returned.

**Note:** If II_DECIMAL is set to comma, be sure that when SQL syntax requires a comma (such as a list of table columns or SQL functions with several parameters), that the comma is followed by a space. For example:

```
SELECT col1, IFNULL(col2, 0), LEFT(col4, 22) FROM t1:
```

## IFNULL Result Data Types

If the arguments are of the same data type, the result is of that data type. If the two arguments are of different data types, they must be of comparable data types. For a description of comparable data types, see Assignment Operations.

When the arguments are of different but comparable data types, the DBMS Server uses the following rules to determine the data type of the result:

- The result type is always the higher of the two data types; the order of precedence of the data types is as follows:

  date > money > float4 > float > decimal > integer > smallint > integer1

  and

  c > text > char > varchar > byte > byte varying

- The result length is taken from the longest value. For example:

  ```
  IFNULL (VARCHAR (5), c10)
  ```

  results in c10.

The result is nullable if either argument is nullable. The first argument is not required to be nullable, though in most applications it is nullable.

## IFNULL and Decimal Data

If both arguments to an IFNULL function are decimal, the data type of the result returned is decimal, and the precision (total number of digits) and scale (number of digits to the right of the decimal point) of the result is determined as follows:

- **Precision**—The largest number of digits to the left of the decimal point (precision - scale) plus largest scale (to a maximum of 39)
- **Scale**—The largest scale
