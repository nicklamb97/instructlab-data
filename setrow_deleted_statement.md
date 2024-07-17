# Setrow Deleted Statement
This statement sets a row deleted in an array.
This statement has the following syntax:

```4gl
exec 4gl setrow deleted array (rownumber = integer);
```

The setrow deleted statement makes a non-deleted array row into a deleted array row.
This statement is similar to the SetRowDeleted Method for the ArrayObject class.
This statement has the following parameters:
### array
- Specifies the handle of the array
### integer
- Specifies the number of the row to be deleted