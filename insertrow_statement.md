# Insertrow Statement
This statement inserts a row into an array.
This statement has the following syntax:

```4gl
exec 4gl insertrow array (rownumber = integer);
```

The insertrow statement adds a new row, containing default data, to the array.
#### Note:  This statement is similar to the InsertRow Method for the ArrayObject class.

This statement has the following parameters:
### array
- Specifies the handle of the array
### integer
- Specifies the number of the row to be added