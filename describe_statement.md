# Describe Statement
The describe statement in Actian 4GL retrieves a list of the attributes of a class or array

## Syntax

```4gl
exec 4gl describe object into descriptor;
```

## Description
The describe statement creates descriptors for the attributes of the specified object or array. These descriptors are stored in the SQLDA structure specified by descriptor. For an array, the describe statement generates descriptors for the type of the array's rows.

## Parameters
object: 
- Specifies the handle of the object or array to be described. This parameter is an integer.
descriptor: 
- Specifies the name of the descriptor for the SQLDA.
