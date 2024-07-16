# Get Attribute Statement
The get attribute statement in Actian 4GL retrieves an attribute from an array or object.

## Syntax
```4gl
exec 4gl get attribute object | array index
          (:var[:ind] = attribute[, :var[:ind] = attribute]);
exec 4gl get attribute object | array index using descriptor;
```

## Description
The get attribute statement copies the attributes of an object or array row into 3GL variables. The named attributes must appear in the object or array, and the types of the attributes must be compatible with the types of the target variables. The using descriptor form gets the attributes indicated in the SQLDA set up by the describe statement.

## Parameters
object: 
- Specifies the handle of the object. This parameter is a 4-byte integer.
array: 
- Specifies the handle of the array. This parameter is a 4-byte integer.
var: 
- Specifies a 3GL variable to receive results.
ind: 
- Specifies a null indicator variable.
attribute: 
- Specifies the name of the attribute.
index: 
- Specifies the array index.
descriptor: 
- Specifies the name of the SQLDA descriptor.