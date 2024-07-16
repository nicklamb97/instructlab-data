# GetRow Statement
This statement gets a row from an array; specifically, it gets the 3GL object handle for a given array member.
This statement has the following syntax:

```4gl
exec 4gl getrow array index (:object = row);
```

This statement has the following parameters:
### array
- Specifies the handle of the array
### index
- Specifies the index number of the variable. It indicates the row of the array to be accessed.
### object
- Specifies a 4-byte handle for the object