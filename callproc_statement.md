## Callproc Statement

This statement calls a procedure from a frame, a procedure, or a method for a user class. The callproc statement calls any of these types of procedures:

- A global or local procedure written in 4GL
- A procedure written in another high-level programming language (a 3GL language such as C or FORTRAN)

**Note:** To call a database procedure, use the Execute Procedure Statement.

This statement has the following syntax:

`[returnvariable =] callproc procname[(parameterlist)];`

You can call this procedure from a frame, a procedure, or a method for a user class. Because `procname` is a dynamic name, you can use an actual procedure name or a variable that resolves to a procedure name at runtime. If you use an actual procedure name, OpenROAD resolves the reference at compile time. If you use a variable, OpenROAD resolves the procedure reference at runtime. This difference in the timing of name resolution has an important consequence when the callproc statement is executed inside an included application.

To pass values to the called procedure, use the optional parameter list in the callproc statement. You can pass the parameters by value or by reference. To pass values by reference to a 4GL or 3GL procedure, use the `byref(variable)` option.

For more information about using the callproc statement to pass parameters by value and by reference, see the *Programming Guide*.

On returning from the called procedure, you can pass a single value back to the calling procedure or frame. The data type of the variable that receives the return value must be compatible with the return type of the called procedure. For database procedures, the return type is always integer.

If you use a variable to represent the procedure name so that the user can specify a procedure at runtime, you must ensure that the data type of the procedure's return value is compatible with that of any specified return variable. OpenROAD assumes that the return value's data type is the same as the return variable type. If the procedure attempts to return a value of an incompatible type, an error results at runtime.

**Note:** You can also call a procedure as part of an expression. When you do so, the callproc keyword is not needed.

### Parameters--Callproc Statement

This statement has the following parameters:

#### returnvariable

Specifies the name of the variable in the calling frame or procedure that receives the return value from the called procedure. Its data type must agree with the return type of the called procedure.

#### procname

Specifies the name of the procedure you are calling. You must enclose any procedure name that is the same as a 4GL keyword in single quotes. The procedure name you use for `procname` must match the name in the procedure's definition.

#### parameterlist

For 4GL procedures, see NamedParameterList.
For 3GL procedures, see UnnamedParameterList.

### Example--Callproc Statement

Call the OpenROAD procedure, addtax, passing the `taxpercent` variable by value and the `price` variable by reference:
`callproc addtax(tax = taxpercent, total = byref(price));`
Because `price` is passed by reference, the procedure can update its value by adding the tax amount. The new total price is visible to the calling frame or procedure after the addtax procedure returns.
