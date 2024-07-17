# Callframe Statement
This statement opens a new frame in a new window.
This statement has the following syntax:

```4gl
[returnvariable =] callframe framename([parameterlist])
          [withclause];
```

The callframe statement transfers control from the current frame to another frame in a new window. The new window covers the window of the calling frame. (The frame that issues the callframe statement is the calling frame, and the frame to which control is transferred is the called frame.) The callframe statement is often used to display dialogs that collect user input relevant to the calling frame.

Because framename is a dynamic name, you can use an actual frame name or a variable that resolves to a frame name at runtime. If you use an actual frame name, OpenROAD resolves the reference at compile time. If you use a variable, OpenROAD resolves the frame reference at runtime. This difference in the timing of name resolution has an important consequence when the callframe statement is executed inside an included application.

Because you can nest called frames, the called frame can call another frame, and so on. You can also call frames recursively.

To exit a called frame, use the return statement. This statement lets you pass a single value back to the calling frame in the return variable. The data type of this variable and the data type of the frame's return value must be compatible. The data types are checked at compile time. (For more information, see the Return Statement.)

You cannot use the callframe statement in an expression.

## Parameters--Callframe Statement
This statement has the following parameters:

### returnvariable
- Specifies the name of a variable in the calling frame or procedure that receives the return value from the called frame. The data type of returnvariable must agree with the return type of the called frame, which is declared in the property sheet when you create the frame.

### framename
- Specifies the OpenROAD name of the frame you are calling. It is a dynamic name.

### parameterlist
- See NamedParameterList.

### withclause
- See WithClause.

## Examples--Callframe Statement
Call newframe, passing a value from the simple variable, projnum, of the calling frame to the simple variable, projnum, in the called frame:

```4gl
callframe newframe(projnum = projnum);
```

Call newframe, placing a value in the status variable upon returning to the calling frame:

```4gl
status = callframe newframe;
```

Call calcframe, passing the simple variable, calc, by reference. On return, the value of calc reflects any changes made to the opexp value by calcframe:

```4gl
callframe calcframe(opexp = byref(calc));
```

Call calcframe, changing the default position of the frame given on its property sheet:

```4gl
callframe calcframe(ofexp = byref(calc))
          with windowxleft = 1000, windowytop = 2000,
          windowplacement = WP_PARENTRELATIVE;
```