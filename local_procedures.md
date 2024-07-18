# Local Procedures

You can use a local procedure in 4GL to code a callable procedure for a single frame, field, procedure, or user class script.

Although you define local procedures in the frame, field, procedure, or user class script in which they are called, you must declare a forward reference before you can define a local procedure. Put the forward reference in the declare block for the script in which the local procedure is defined.

You define local 4GL procedures directly in the frame, field, procedure, or user class script using the procedure statement as described in the following section.

Use the callproc statement to call a local procedure defined in the current frame or procedure. For more information see the *Language Reference Guide* online help.

## Declaring Forward References for Local Procedures

Use one of the following formats when declaring the local procedure forward reference:

```
localprocname = procedure returning typeofreturnvalue
localprocname = procedure returning none
localprocname = procedure
```

The `procedure returning none` and the `procedure` formats are equivalent. They specify that the procedure does not return a value.

## How You Can Define Local Procedures

Use the following syntax to define a local procedure:

```
procedure procname [([parameterlist])] = 
[declare
    localvariablelist
[enddeclare]]
begin 
    statementlist
end
[;]
```

For more information about the *parameterlist*, see Procedure Statement.

For more information about the declare block, see Initialize Statement.

The runtime system searches local scopes of the currently executing frame or procedure and executes the local procedure if it is in the callproc string variable.

## Example--Local Procedure

This section provides an example of a procedure definition and a sample call to the procedure. Here is the procedure definition:

```
procedure addtax (tax=float8, price=float8) = 
{
    return (price * tax);
}
```

The name of the sample procedure is `addtax`. It has two parameters, `tax` and `price`. The data types of each are `float8`. The caller passes information to the procedure using these parameters. When you call the procedure, it uses the parameter values it receives from the caller in its calculations and returns the result to the caller.

The following is a sample statement calling the addtax procedure:

```
tax = callproc addtax (tax = taxpercent, 
    price = costfield);
```

The order in which the parameters are specified in the callproc statement need not match the order in which they appear in the procedure's heading. However, they must be identical in name to the parameters in the procedure definition. Here is a second call to the same procedure:

```
cost = currprice + addtax (tax = .1, price = currprice);
```
