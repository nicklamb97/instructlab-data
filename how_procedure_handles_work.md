# How Procedure Handles Work (ProcHandle Objects)

A *procedure handle* (ProcHandle object) represents a global or local procedure and the scope in which to execute it. ProcHandle objects have several uses, for example:

- They let called frames call procedures declared locally in their calling frame.
- They let field scripts declare local procedures that can be called from anywhere in the frame.

ProcHandle objects are useful to pass procedures to contexts in which the procedure is not otherwise visible. They are used similarly to function pointers in the C language.

For example, you could write a field script that defines its own local cleanup procedures. In its initialize block it could insert its handles into an array of cleanup procedure handles defined at the frame level. You can create local cleanup procedures for other field scripts as well. Later, at cleanup time, the frame can loop through the cleanup array and use the handle to call each procedure.

## How You Can Create a ProcHandle Object

To obtain a ProcHandle object, invoke the GetProcHandle method on a Scope object. The syntax is:

```
prochandle = scope.GetProcHandle(name = string);
```

**string**
Specifies a literal string or a string variable that specifies the name of a procedure.

**scope**
Specifies where the procedure:
- Is visible
- Is to be executed

Generally, the scope of a ProcHandle object is CurScriptScope (the context of the currently executing field script) or CurFrame.Scope. To create a ProcHandle object for a global procedure named identically to a local procedure, use a global scope to represent the current application (CurSession.Scope).

If the currently executing frame or procedure is part of an included application, CurSession.Scope represents the global scope for that included application and the global scopes for all applications directly included by that application.

The names of included applications can be specified explicitly. For example, the following code fragment gets the procedure handle for a procedure named procname defined for an application named appname. The procedure handle is put into a variable named handle:

```
handle = CurSession.Scope.GetProcHandle(Name = 'appname!procname');
```

**Note:** appname must reference an application directly included by the currently active application.

## How You Can Execute a ProcHandle Object

To execute the procedure specified by the ProcHandle object, invoke the Call method defined for the ProcHandle class. The syntax is:

```
[retval =] prochandle.Call(parameters);
```

The *parameters* are specified just as in a callproc statement. For an explanation about using the callproc statement, see How You Can Pass Parameters to 4GL Procedures.

The Call method uses the scope specified by the ProcHandle object to execute it.

## Restrictions to Using the Call Method

There are some restrictions to using the Call method. This method can be invoked only in the following places:

- In a statement by itself
- As the right side of an assignment

The Call method can never be invoked inside an expression.

If a procedure handle represents a local procedure that was defined in a frame or global procedure component, there are two additional restrictions on when the Call method can be invoked on the procedure handle:

- The Call method cannot be invoked after the frame or global procedure in which it was defined (and which contains the scope on which the GetProcHandle was issued) has terminated.
- The Call method cannot be invoked from a "thread" other than the one in which the GetProcHandle was issued.

For example, a frame can create a procedure handle on one of its local procedures and pass it as a callback routine to a called frame, but not to an opened frame (because the openframe statement creates a new thread).

These restrictions do not apply to a procedure handle that represents a global procedure or a local procedure defined in a user class script.
