# Scalar Functions

The scalar functions require either one or two single-value arguments. Scalar functions can be nested to any level.

The types of scalar functions are as follows:

- Data type conversion
- Numeric
- String
- Date
- Hash
- Random number

**Note:** If II_DECIMAL is set to comma, be sure that when SQL syntax requires a comma (such as a list of table columns or SQL functions with several parameters), that the comma is followed by a space. For example:

```sql
SELECT col1, IFNULL(col2, 0), LEFT(col4, 22) FROM t1:
```
