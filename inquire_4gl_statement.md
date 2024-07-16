# Inquire_4gl Statement
This statement gets object or array information.
This statement has the following syntax:

```4gl
exec 4gl inquire_4gl(variable[:ind] = 4gl_constant [(array)]);
```

The inquire_4gl statement gets information about an object or array. The inquire_4gl statement can return:
- The name of the object's object type (record type name)
- Whether the named structure is an array
- Error status from the previous operation
- Error text from the previous operation
- Information about the number of rows in the array

This statement has the following parameters:
### variable
- Specifies a 3GL variable
### array
- Specifies the handle of the array
### 4gl_constant
- Specifies a constant containing the information you are seeking from 4GL about the object or array. These constants are described in the table that follows.
### ind
- Specifies a null indicator variable.

The following table describes the values that can be used in place of 4gl_constant in the syntax:
### allrows
- Integer, the total number of rows in the specified array, including deleted rows.
- This constant is similar to the AllRows method for the ArrayObject class.

### firstrow
- Integer, the index of the first (lowest-numbered) deleted row.
- This constant is similar to the FirstRow method for the ArrayObject class.

### lastrow
- Integer, the number of non-deleted rows in the specified array.
- This constant is similar to the LastRow method for the ArrayObject class.

### IsArray
- Integer, returns one if an array, zero if not.

### RecordTypename or ClassName
- Character, for an object, the name of the object type. For an array, this value is the name of the row type.

### errorno
- Integer, the number of the error for the previous operation. It is zero if OK.

### errortext
- Character, the text associated with the error for the previous operation.