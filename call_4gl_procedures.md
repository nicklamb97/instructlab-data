# How You Can Call 4GL Procedures

The callproc statement lets you call a global 4GL procedure from any OpenROAD script or another 4GL procedure. You also use the callproc statement to call a local procedure defined in the current frame or procedure.

## Basic Syntax

In the simplest version of the statement, you can call a procedure with no return value or parameters. The basic syntax is:

```sql
callproc procedurename
```

### *procedurename*

Specifies the name that you gave to the 4GL procedure when you created it. You can enter the procedure name directly or you can use a variable to specify the procedure name dynamically. Using a variable lets you specify the procedure name at runtime.

Example:
```sql
callproc error_handler;
```

When the procedure returns a value, you must specify a variable in the calling frame or procedure to receive the return value. The return variable must be the same data type as the return value. The basic syntax is:

```sql
[return_variable =] callproc procedurename
```

## How You Can Pass Parameters to 4GL Procedures

To pass a value to a 4GL procedure in the callproc statement, you specify the name of the parameter in the 4GL procedure that is to receive the value. The basic syntax is:

```sql
[return_variable =] callproc procedurename [(parameter = expression {, parameter = expression})]
```

### *parameter*

Assigns the expression in the callproc statement to the parameter in the called procedure that you declared with the procedure statement. For more information about declaring procedure parameters, see the *Language Reference Guide* online help.

Because you specify the parameters by name rather than by position, you can pass them in any order, and you need not pass all the parameters.

### *expression*

Specifies a constant or any legal OpenROAD expression, as long as the resulting data type is compatible with the data type of the local variable in the procedure. Variables in the expression can be any type, including reference variables and array variables.

Example:
```sql
callproc remove_new_detail(new_details_frames = new_details_frames, details_frame = video.details_frame);
```

Any variable in the procedure that you do not specify in the parameter list is set to its default value.

Parameters to 4GL procedures can be passed by value and by reference. For a discussion of passing parameters by value and by reference, see How You Can Pass Parameters Between an Active Child and Inactive Parent.

The value of the referenced variable is not updated until the called procedure returns. At this point, the variable in the calling frame is updated the same way it would be if you had used an assignment statement. If a field on the form is associated with the variable, the field does not display the new value until the current event block completes. To cause the value to update before the end of the event block, you can use the Flush method immediately after the callproc statement. For more information about the Flush method, see the *Language Reference Guide* online help.
