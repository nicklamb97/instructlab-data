# The `return` statement in Actian 4GL

The return statement in Actian OpenROAD 6.2 is used to close the current frame or called procedure and return to the calling frame or procedure. It can also pass a value back to the caller.

## Syntax
```4gl
return [expression];
```

## Description
The return statement:
- Closes the current frame, procedure, or method.
- Returns the user to the calling frame, procedure, or method.

## Behavior:
### Frame Called by callframe:
- Closes the frame.
- Returns control to the calling frame, procedure, or method at the statement following the callframe statement.
### Frame Initiated through openframe:
- Closes the frame.
- User must initiate interaction with an active frame.
### Returning from Called Procedure/Method:
- Passes control back to the calling frame, procedure, or method.
- Resumes execution at the statement following the callproc statement or method invocation.
### Parent of Opened Frames:
- Sends a Terminate event to these frames and then closes them.
### All Active Frames/Procedures are Children:
- Closes the entire application, functioning as an exit statement.

## Passing Values
The return statement can pass a value (such as a return status) back to the calling frame, procedure, or method. For a frame called with openframe, any return value is ignored.

## Parameters
expression: Specifies a value returned to the calling frame or procedure. The value must be compatible with the return data type of the called frame, global procedure, or method as defined in the visual development environment.

## Example
At the end of an operation, return control to the calling frame, passing back the value in the status variable:

```4gl
on click file.end =
begin
    return status;
end
```

## Notes
The return statement is essential for controlling the flow of execution between different frames, procedures, and methods.
It ensures proper closure of frames and passing of control and values back to the caller.
