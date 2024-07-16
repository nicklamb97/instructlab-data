# Set Attribute Statement
This statement sets one or more attributes of an object or array.
This statement has the following syntax:

```4gl
exec 4gl set attribute object | array index
          (attribute = :var[:ind])
          [(attribute = :var][:ind]])];
exec 4gl set attribute object | array index using descriptor;
```

The set attribute statement copies the values from 3GL variables into the attributes of an object or array row. The named attributes must appear in the object or array, and the types of the attributes must be compatible with the types of the variables.

The using descriptor form sets the attributes indicated in the SQLDA set up by the describe statement. For more information, see Describe Statement.

This statement has the following parameters:

### object
- Specifies the handle of the object
### array
- Specifies the handle of the array
### index
- Specifies the index number of the variable. It indicates the row of the array variable to be set.
### attribute
- Specifies the name of the attribute
### var
- Specifies a 3GL variable
### ind
- Specifies a 3GL null indicator
### descriptor
- Specifies the name of the SQLDA descriptor