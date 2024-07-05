# Field and Menu Item Variables

Field and menu item variables are variables that are associated with the *value* in a field or menu item. These 
variables are not associated with the object represented by the field or menu item.

When you create a field or menu item in OpenROAD Workbench, you generally specify a variable name on the Property 
Inspector. This variable is associated with the data displayed in the field. You can reference this variable in your 
4GL code whenever you need to access the data in the field.

For example, assume that you create an entry field and enter the variable name as *title*. You can set the value in 
that field with the following statement:

```
title = 'Destry Rides Again';
```

It is not necessary to declare field and menu item variables in your scripts; OpenROAD declares them automatically 
when you create the field or menu item in OpenROAD Workbench.

To reference the object represented by a field or menu item instead of its value, use the FIELD() function, as 
described in Field Function, with the field or menu item variable. For example, the following statement changes the 
background color of the previously described entry field:

```
FIELD(title).BgColor = CC_ORANGE;
```

For more information about the field function, see Field Function.

# How You Can Initialize Variables

When OpenROAD starts an application, any global variables associated with the application are initialized. Similarly, 
when you call a frame or procedure, OpenROAD initializes the variables associated with the frame or procedure.

OpenROAD uses the default value of the variable as the initial setting. For field and menu items or global variables, 
you can define this value in the OpenROAD Workbench. If you do not, OpenROAD uses the system defaults. OpenROAD also 
uses the system default for variables declared in the initialize statement of a frame script.

For simple variables, the system defaults are zero for numeric data types, or the empty string ('') for non-nullable 
character data types. However, you can overwrite these defaults using the default clause, for example:

```
i = integer not null default 3;
```

For reference variables, OpenROAD creates an object of the declared class and sets the reference variable to 
reference that object. The attributes of this object are initialized to their default values (which are specified in 
the class definition). For attributes that are reference variables, you can set the initial value of the variable to 
null. In this case, no object is created for the reference variable.
