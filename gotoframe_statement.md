## Gotoframe Statement

This statement passes control from the current frame to another frame.

This statement has the following syntax:
```
gotoframe framename([parameterlist])
          [withclause];
```

The gotoframe statement calls a specified frame to run within another window and then closes the calling frame and its window. The gotoframe statement functions like the callframe statement, except that generally no return to the current frame is expected. If the current frame has been called from another frame that expects a return value, then the last frame called by a gotoframe statement must complete the call sequence and provide the return value. The return type of the called frame must be the same as that of the calling frame.

Because `framename` is a dynamic name, you can use an actual name or a variable that resolves to a frame name at runtime. If you use an actual frame name, OpenROAD resolves the reference at compile time. If you use a variable, OpenROAD resolves the frame reference at runtime. This difference in the timing of the name resolution has an important consequence when the gotoframe statement is executed inside an included application.

The parameter list lets you pass values to the called frame. These values are usually supplied by displayed or local variables in the calling frame. The values can also come from constants or any legal 4GL expression.

You cannot use the byref clause in a gotoframe statement.

The gotoframe statement does not send a Terminate event to the frame issuing the statement (the frame that is closing).

For information about the methods for calling frames, see Callframe Statement and Openframe Statement.

### Parameters--Gotoframe Statement

This statement has the following parameters:

#### framename

Specifies the name of the frame you are calling. It is a dynamic name.

#### parameterlist

See NamedParameterListWithoutByref.

#### withclause

See WithClause.

### Example--Gotoframe Statement

Call the frame, nextframe, and close the current frame:

```
on click file.transfer =
begin
     gotoframe nextframe(sequence = 3);
end
```
