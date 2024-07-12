# Method Statement

This statement defines a 4GL method.

This statement has the following syntax:

```
method methodname [(parameterlist)] = [declareblock] beginendblock[;]
```

The method statement defines a 4GL method.

The *parameterlist* and *declareblock* define local variables for the method. Parameters are local variables that can be specified as parameters on a method invocation statement that invokes the method. The data type of a local variable can be any simple data type, a system class, or user class. The *beginendblock* specifies the actions to be performed by the method.

For more information about using methods, see the *Programming Guide*.

## Parameters--Method Statement

This statement has the following parameters:

**methodname**

Specifies the name of the method. For a global method, this name must be the one that you specified in the Class Editor.

**parameterlist**

See NamedParameterListDefinition.

**declareblock**

See DeclareBlock.

**beginendblock**

See BeginEndBlock.

## Example--Method Statement

Define a method that adds the appropriate tax to an attribute "amount":

```
method addtax
(tax = float) =
begin
CurObject.amount = CurObject.amount + CurObject.amount*tax;
end;
```
Define a method that displays a message containing the FirstName attribute of the object in the trace window (no parameters needed):
```
method printmsg =
begin
CurMethod.Trace(text = CurObject.FirstName);
end;
```
