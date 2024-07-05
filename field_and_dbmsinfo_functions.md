# Field Function

Note: The field function is supported in 4GL but not SQL.

The field function gives you access to the attributes of the object associated with a displayed field or menu item, 
rather than to the actual value of the field or menu item. For example, assume that you create a frame with a slider 
field. In the OpenROAD Workbench Property Inspector for the slider field, you specify the field's variable as 
temperature. This variable is a simple integer variable. When you reference the variable in a program, the reference 
provides the current value in the slider field:

```
if temperature >= 85 then ...
```

In contrast, if you use the FIELD() function with the variable name, then the reference is to the object that the 
field represents, the SliderField object. Using the FIELD() function lets you read or change attributes of that 
object:

```
FIELD(temperature).TypeFace = TF_LUCIDA;
```

This statement changes the type face of the value settings on the slider field.

For more examples and further discussion of the field function, see Creating Dynamic Frames in the Programming Guide.

# Dbmsinfo Function

In OpenROAD, if you use the dbmsinfo function as a statement by itself, the only valid request_name is _bintim:

```
variable = dbmsinfo('_bintim')
```

All other values of request_name return blanks unless the function is part of a larger SQL statement. For example, 
the following statement returns a user name:

```sql
select logname = dbmsinfo('username')
```

However, the following statement returns blanks:

```
logname = dbmsinfo('username')
```
