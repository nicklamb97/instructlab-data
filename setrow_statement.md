# Setrow Statement
This statement sets a row in an array.
This statement has the following syntax:

```4gl
exec 4gl setrow array index (row = :object);
```

This statement sets a given row in an array to point at a particular object.
This statement has the following parameters:
### array
- Specifies the handle of the array
### index
- Specifies the index number of the variable. It indicates the row of the array to be accessed.
### object
- Specifies a 4-byte handle for the object