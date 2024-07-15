# Openframe Statement

This statement opens a new frame to run concurrently with the calling ProcExec.

This statement has the following syntax:
```
[GhostExec_object =] openframe framename ([parameterlist]) [withclause];

```
The openframe statement opens a new frame in a separate OpenROAD thread that runs concurrently with the calling ProcExec.

Because the value of *framename* is a dynamic name, you can use an actual frame name or a variable that resolves to a frame name at runtime. If you use an actual frame name, OpenROAD resolves the reference at compile time. If you use a variable, OpenROAD resolves the reference at runtime. This difference in the timing of name resolution has an important consequence when the openframe statement is executed inside an included application.

You can send the GhostExec object reference of the calling GhostExec or FrameExec (the CurFrame variable) as a parameter to the opened frame so that it can communicate with the calling frame.

You may nest opened frames. That is, a called frame can call or open another frame, and so on. You also cannot use the openframe statement in an expression.

For more information about communicating between frames, see the *Programming Guide*.

## Parameters--Openframe Statement

This statement has the following parameters:

**GhostExec_object**

Specifies a reference variable of type GhostExec. It is assigned the GhostExec object of the opened frame.

If the calling ProcExec is a GhostExec or FrameExec, it can use the value of the GhostExec_object, which is the return value of the openframe invocation, to continue to communicate with the opened frame. Communication is accomplished with the SendUserEvent Method.

For more information about using the SendUserEvent method to communicate between frames, see the *Programming Guide*. For more information about different ways to call frames, see Callframe Statement and Gotoframe Statement.

**framename**

Specifies the name of the frame you are opening. This name is a dynamic name.

**parameterlist**

See NamedParameterListWithoutByref.

**withclause**

See WithClause.

You can set the ParentFrame attribute using the *withclause*. To specify this attribute, *value* must be a reference variable that points to a ProcExec object for a currently running frame or null. If it is a reference variable pointing to a FrameExec object, then the called frame is considered a child of the frame represented by that FrameExec object. If you assign null to the ParentFrame attribute, then the called frame runs as a detached frame, independent of other frames in the application. If you do not set the ParentFrame attribute, then the called frame is the child of the calling ProcExec.

## Examples--Openframe Statement

Open the show_details frame and send a user event message:

```
initialize =
declare
testarray = array of Addr;
detailframe = FrameExec default null;
enddeclare
begin
detailframe = null;
/* other statements */
end
on click menu.detail =
begin
detailframe = openframe show_details (addr_data = testarray);
/* as soon as show_details frame is open,
** control returns here */
/* other statements */

end
on click menu.sendtodetail =
begin
if detailframe is not null then
detailframe.SendUserEvent (eventname 'messagefromopener');
endif;
end
```
