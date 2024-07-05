# Abstract Data Types

Abstract data types include the following:
- Money
- Numeric string
- Logical

## Money Data Type

The money data type is an abstract data type. Money values are stored significant to two decimal places. These values 
are rounded to their amounts in dollars and cents or other currency units on input and output, and arithmetic 
operations on the money data type retain two-decimal-place precision.

Money variables can accommodate the following range of values:
$-999,999,999,999.99 to $999,999,999,999.99

A money value can be specified as either:
- A character string literal—The format for character string input of a money value is $sdddddddddddd.dd. The dollar 
sign is optional and the algebraic sign(s) defaults to + if not specified. There is no need to specify a cents value 
of zero (.00).
- A number—Any valid integer or floating point number is acceptable. The number is converted to the money data type 
automatically.

On output, money values display as strings of 20 characters with a default precision of two decimal places. The 
display format is:
$[-]dddddddddddd.dd

where:
$ is the default currency symbol
d is a digit from 0 to 9

The following environment settings affect the display of money data. For details, see the Workbench User Guide:

| Variable | Description |
|----------|-------------|
| II_MONEY_FORMAT | Specifies the character displayed as the currency symbol. The default currency sign is the dollar sign ($). II_MONEY_FORMAT also specifies whether the symbol appears before of after the amount. |
| II_MONEY_PREC | Specifies the number of digits displayed after the decimal point; valid settings are 0, 1, and 2. |
| II_DECIMAL | Specifies the character displayed as the decimal point; the default decimal point character is a period (.). II_DECIMAL also affects FLOAT, FLOAT4, and the DECIMAL data types. |

Note: If II_DECIMAL is set to comma, be sure that when SQL statements within OpenROAD require a comma (such as a list 
of table columns or SQL functions with several parameters), that the comma is followed by a space. For other 
statements used in OpenROAD, use II_4GL_DECIMAL. For more information, see the Workbench User Guide. For example:

```sql
select col1, ifnull(col2, 0), left(col4, 22) from t1;
```

## Numeric String Data Type

The numeric string data type is a virtual type that exists only when numeric data types are compared directly with 
character data. If a comparison is requested between these two classes of data, the character data is examined to see 
if it is numeric in form. If it is, then the comparison is performed as though both were numeric. If the data is not 
numeric, the result will be such that all numbers collate before all non-numbers. For example:

–100 < '-9' < '0.01' < 1 < '1.1e1' < 'one' < 'three' < 'two'

The numeric conversion is performed using float8 precision and leading and trailing spaces are ignored.

The data types considered as numeric are all the integer types, all the float types, and decimal. Money is not 
treated in this manner because it has its own character data compatibility.

To be deemed numeric, a character data value must contain only one number, which can be in integer, decimal, float, 
or scientific notation form. All the character data types are checked for numeric content except the long variants, 
which are treated as non-numeric.

## Logical Key Data Type

The logical key data type allows the DBMS Server or your application to assign a unique key value to each row in a 
table. Logical keys are useful when an application requires a table to have a unique key, and the columns of the 
table do not comprise a unique key.

There are two types of logical keys:
1. SYSTEM_MAINTAINED
2. NOT SYSTEM_MAINTAINED

Specify the scope of uniqueness for system maintained logical key columns using the following options:
- TABLE_KEY: Values are unique within the table.
- OBJECT_KEY: Values are unique within the database.

TABLE_KEY values are returned to embedded SQL programs as 8-byte strings, and OBJECT_KEY values as 16-byte strings. 
Values can be assigned to logical keys that are NOT SYSTEM_MAINTAINED using string literals. For example:

```sql
INSERT INTO keytable(table_key_column) VALUES('12345678');
```

Values assigned to TABLE_KEYs must be 8-byte strings; values assigned to OBJECT_KEYs must be 16-byte strings.

In a UTF-8 environment, logical keys must be passed as type BYTE.

### Restrictions on Logical Keys

When working with logical keys, be aware of the following restrictions:
- A system_maintained logical key column cannot be created using the CREATE TABLE...AS SELECT statement. A not 
system_maintained data type is assigned to the resulting column.
- The COPY statement cannot be used to load values from a file into a system_maintained column.
