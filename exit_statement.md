# Exit Statement

This statement closes the application.

This statement has the following syntax:

```
exit;
```

The exit statement closes an application and returns control to the location where the application was originally entered. This statement terminates all open frames.
Before the application finally exits, the exit statement sends each open frame a Terminate event. You can use this feature to ensure a clean application shutdown by placing in each frame an event block that activates on the Terminate event and contains any necessary shutdown operations.
For more information about closing applications, see Terminate Event. See also the SetExitTrap Method, which provides the functionality to interrupt normal exit statement handling.
