# On Statement

This statement specifies an event block for one or more events.

This statement has the following syntax:
```
on eventtype [fieldname] | ['eventname'] [(parameterlist)]
{, on eventtype [fieldname] | ['eventname'] [(parameterlist)} =
[declareblock]
beginendblock[;]
```
The on statement specifies the start of an event block that is triggered by one or more events.

Although the values of `fieldname` can be any field or menu item on a frame, do not use the field function with the field or menu item name. If you do not specify a field name and the on statement is in the frame script, the value of `fieldname` defaults to the frame. If the on statement is in a field or menu item script, the value of `fieldname` defaults to that field or menu item.

Include a value for `eventname` only if the event type is userevent, extclassevent, or dbevent. If you do not specify an event name in the event block, OpenROAD executes the block whenever the frame receives any user event or database event, respectively.

For more information about using user events and database events, see the following events:

- DBEvent Event
- ExtClassEvent Event
- UserEvent Event

With user event and external class event activations, you may supply an optional parameter list. The parameter list defines locally, within the given activation, the parameters that will be set when this activation occurs. The parameters are set to the values prescribed when the event was originally sent.

## Parameters--On Statement

This statement has the following parameters:

**eventtype**

Specifies an event type. The type depends on the object being used.

**eventname**

Specifies a user event name, database event name, or external class event, depending on whether you use the on userevent, the on dbevent, or the on extclassevent statement. In any case, eventname is optional. If used, the event name can be any string of up to 32 characters, surrounded by single quotes.

**fieldname**

Specifies the name of the field or menu item that triggers the event. This name is the name that you gave the field or menu item when you created it in OpenROAD Workbench. Use this parameter with all event types, except database events and user events.

**parameterlist**

See NamedParameterListDefinition. Defines parameters for the event. You can specify them as parameters on a SendUserEvent method invocation (see GhostExec (see SendUserEvent Method)) or provide them through the external event (for an ExtClassEvent). The data type of the parameter can be any simple data type, a system class, or user class.

**declareblock**

See DeclareBlock (see BeginEndBlock).

**beginendblock**

See BeginEndBlock.

## Sending User-named Parameters with Userevents

You can specify user-named parameters as arguments to the SendUserEvent method defined for the GhostExec class. To do so, you can specify, at the time a userevent is triggered, which arguments in the target frame's userevent activation block declaration will be set with values from the frame sending the event. You can choose from any of the supported OpenROAD 4GL data types, and pass as many arguments as you want from the source frame to the event target.

This support for user-named parameters supplements the existing support for the predefined attributes MessageFloat, MessageInteger, MessageObject, and MessageVarchar.

## Examples--On Statement

Return from a frame when the user clicks the Return button:
```
on click return_button =
begin
return;
end
```
The following examples show how you might send a user-named parameter to a target frame through the use of a SendUserEvent invocation.

The activation block in the userevent sending frame is:
```
on click =
declare
recframe = FrameExec default null;
emp_loc = varchar(256) not null default 'New Haven';
enddeclare
begin
/* other code */
recframe = OpenFrame EmployeeMaint();
recframe.SendUserEvent(eventname = 'NewEmp',
employee_location = emp_loc);
end
```
The Target Frame's activation block for the NewEmp event is:
```
on userevent 'NewEmp' (
employee_location = varchar(32) not null
)=
begin
Message 'Adding employee to the '+ employee_location + ' location...';
end
```
If there is a data type mismatch between the parameter being passed and the variable receiving the parameter in the receiving frame, it is flagged with the appropriate error at runtime when the event recipient's activation block is activated. Data type mismatches between the source and target are not flagged by the 4GL compiler.
