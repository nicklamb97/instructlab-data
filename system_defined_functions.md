# System-defined Functions

There are four types of SQL functions:

1. Ingres scalar SQL functions
2. Aggregate functions
3. The ifnull() function
4. The dbmsinfo() function

## Ingres Scalar SQL Functions

These can be used in all OpenROAD statements and in all SQL statements that are used locally within OpenROAD, except for object_key() and table_key().

## Aggregate Functions

These take a set of values (for example, the contents of a column in a table) as their arguments and can only be used in OpenROAD within SQL statements.

## The ifnull() Function

## The dbmsinfo() Function

OpenROAD can access other database functions used by Ingres, MSSQL, Oracle, and so on, as well as UDFs (user-defined functions) by using Execute Immediate Statement and Direct Execute Immediate Statement.

Programmers should be mindful of the implications of running a function on the client side versus the database side. For example:

```
ld_date = DATE('now'); // returns the client current date and time
SELECT :ld_date = DATE('now'); // returns the server current date and time
```

Generally, the rules for parameters and return values are the same for the 4GL and SQL functions of the same name. If in doubt, see the Ingres SQL Reference Guide or the Enterprise Access documentation for specifics. Here is an example demonstrating slightly different syntax due to the hexadecimal data type indicator (note the capitalization of X/x).

OpenROAD SQL statement:
```sql
SELECT :lv_searchString = HEX(BIT_ADD(BYTE(X'C8'), BYTE(X'5A')));
```
result: 22

Ingres SQL statement:
```sql
SELECT :lv_searchString = HEX(BIT_ADD(BYTE(x'C8'), BYTE(x'5A')));
```
result: 22

For more information about OpenROAD functions, see Function Libraries.

## Supported and Unsupported Functions

OpenROAD supports the non-SQL field() function. OpenROAD does not, however, support many Ingres scalar functions. Although you can use them in SQL statements, OpenROAD cannot process them locally.

Most OpenROAD 4GL functions share the same name and functionality as the equivalent SQL function. The compiler allows the functions to be used both in 4GL programming and in standard SQL statements sent to the connected database. A few functions apply only to SQL statements, and some functions are available in 4GL code only. Here are some examples:

- The string function upper() can be used in both 4GL and SQL.
- The aggregate function count() may be used only in SQL statements.
- The field() function is available only in 4GL code.
- The string function replace() currently is available only in SQL statements. OpenROAD runs this SQL SELECT on the server side, and returns the result to the OpenROAD variable:

```sql
select :lv_searchString = REPLACE('I like bananas','bananas','apples');
```

For more information about OpenROAD functions, see Function Libraries.
