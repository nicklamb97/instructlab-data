# Field Function

The field function gives you access to the attributes of the object associated with a displayed field or menu item, rather than to the actual value of the field or menu item. 

For example, assume that you create a frame with a slider field. In the OpenROAD Workbench's Property Inspector for the slider field, you specify the field's variable as temperature. This variable is a simple integer variable. When you reference the variable in a program, the reference provides the current value in the slider field:

```
if temperature >= 85 then ...
```

In contrast, if you use the field function with the variable name, then the reference is to the object that the field represents, the SliderField object. Using the field function lets you read or change attributes of that object:

```
field(temperature).TypeFace = TF_LUCIDA;
```

This statement changes the type face of the value settings on the slider field.

For more examples and further discussion of the field function, see "Creating Dynamic Frames" in the *Programming Guide*.
