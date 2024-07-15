# Method Invocation Statement

This statement invokes a method that is defined on a class of a reference variable.

This statement has the following syntax:

```
[returnvariable =] referencevariable.method(parameterlist);

```
The method invocation statement calls a method defined for the class of a reference variable. This statement is used for methods of user classes, system classes, and external classes.

You can invoke any method defined for the class of the reference variable or any inherited method from the variable's superclasses. User classes can invoke any method defined for the Object class, because this system class is the superclass for all user classes.

The parameters for a user class or system class method can be optional, and you can specify them in any order. The parameter names are dynamic names and can be specified at runtime. See the description of the specific method for the default behavior for the parameters. You must include the parentheses even if no parameters are included.

**Note:** You can also invoke a method as part of an expression.

## Parameters--Method Invocation Statement

This statement has the following parameters:

**returnvariable**

Specifies the name of a simple variable or reference variable in the calling frame or procedure that receives the return value from the method. The data type of the return variable must match the return type of the method.

**referencevariable**

Specifies the name of a reference variable or an expression that resolves to a reference variable. The class of this reference variable must be of the same class for which the method is defined or a subclass of that class.

**method**

Specifies the name of the method to invoke. This is a dynamic name.

**parameterlist**

For methods of system classes and user classes, see NamedParameterList.
For methods of external classes, see UnnamedParameterList.

## Examples--Method Invocation Statement

Write out a StringObject to a file:
```
method exmpl (
outputtext = varchar(256) not null;
filename = varchar(256) not null;
) =
declare
strobj = StringObject;
retval = integer not null;
enddeclare
begin
strobj.Value = outputtext;
retval = strobj.WriteToFile(filename = filename);
if retval != ER_OK then
message 'Error writing file.';
endif;
end
```
Invoke a method on an option field called statechoices, with no parameters and no return value:
```
field(statechoices).UpdchoiceList();
```

Use a dynamic name to set the value of an attribute of the user-defined class Addr:

```
method exmpl () =
declare
attname = varchar(32) not null;
empaddr = Addr;
value = varchar(20) not null;
enddeclare
begin
empaddr.City = 'New York';
attname = 'city';
value = 'Chicago';
empaddr.SetAttribute(:attname = value);
message 'value is' + empaddr.City;
/* It will be Chicago /
empaddr.City = 'New York';
empaddr.GetAttribute(:attname = byref(value));
message 'value is ' + value;
/ It will be New York */
end
```


