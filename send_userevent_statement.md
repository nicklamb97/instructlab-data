# Send Userevent Statement
This statement sends a userevent to a frame.
This statement has the following syntax:

```4gl
exec 4gl send userevent frame (eventname = :strvar
                              [, messageobject = :objvar]
                              [, messageinteger = :intvar]
                              [, messagefloat = :fltvar]
                              [, messagevarchar = :strvar]
                              [, focusbehavior = :strvar]
                              [, delay = :fltvar]);
```

This statement sends a userevent with a given name to the specified frame with optional parameters. The four optional message parameters messageobject, messageinteger, messagefloat, and messagevarchar let you pass values to the receiving frame. OpenROAD stores these values in the receiving frame's FrameExec object attributes of the same name.

The focusbehavior defines what validation behavior should take place when the userevent is processed (that is, FT_SETVALUE, FT_TAKEFOCUS, or FT_NOSETVALUE). The delay specifies the number of seconds to wait before triggering the userevent.

This statement has the following parameters:
### frame
- Specifies the object handle of the frame this userevent is being sent to
### eventname
- Specifies the name of the userevent to be sent
### strvar
- Specifies a character string value
### objvar
- Specifies a 4-byte object handle
### intvar
- Specifies an integer variable
### fltvar
- Specifies a floating point variable