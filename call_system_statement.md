## Call System Statement

This statement calls the operating system.

This statement has the following syntax:

```
call system [systemcommand];
```
The call system statement calls the operating system from an application. When control returns to the application, execution resumes with the statement immediately following the call system statement. The call system statement can return status information to the application (see the SessionObject class attribute CallSystemStatus Attribute).

When the called application blocks the calling application, there is no direct communication method between the OpenROAD application and the called application. For indirect communication, you can write out files that can be read by the other application or put data in the database that can be read in by the other application.

Although you can use the call system statement to start a second application, you can start a new OpenROAD application more efficiently by using the call runimage statement.

See the Call Statement for information about using the call runimage statement.

### Parameters--Call System Statement

This statement has the following parameter:

#### systemcommand

Specifies a string expression that must evaluate to a single operating system command. If you specify systemcommand, the statement executes the command and returns automatically to the application, either immediately after starting the command (if CurSession.ProcessWait=FALSE) or after the command has finished (if CurSession.ProcessWait=TRUE). In the latter case the application can inquire the status of the executed command by using the CurSession.CallSystemStatus attribute. If you do not specify a system command, the statement puts the user at the operating system prompt (if CurSession.ProcessWindow=TRUE). The user can then execute any operating system commands and return to the application by entering the exit command.

### Example--Call System Statement

Call the operating system to execute the command specified in the cmdname field on the current form:

```
call system cmdname;

```
