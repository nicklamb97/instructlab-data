# Frame Scripts

Each frame can have one frame script. In the frame script, you can do the following:

- Initialize local variables for the frame
- Perform any start-up operations for the frame
- Include event blocks for events that apply to the frame as a whole
- Include event blocks for events on individual fields or menu items

## General Syntax

The general syntax of a frame script is:

```
[initialize [([parameterlist])] =
[declare
        [localvariablelist]
        [localprocedureforwardreferences]
[enddeclare]]
[begin
         statementlist
end [;]]
{eventblock [;]}
{localprocedure [;]}
{parameterlist [;]}
{localvariablelist [;]}
```

### parameterlist

Specifies the following:
`{variable = datatype {, variable = datatype}}`

### localvariablelist

Specifies the following:
`{variable = datatype;}`

The initialize statement contains the local variable definitions and start-up operations for the frame. The event blocks contain the code for the events for the frame, form fields, and menu items. You can include comments at any point in your script. For more information, see Comments.

Most applications have a frame script for each frame. However, if the frame needs no initialize statement, you can create the field and menu scripts separately and omit the frame script.

## Initialize Statement

When the optional initialize statement is used, it must be the first statement in the frame script.

The initialize statement can have a parameter list that declares the parameters of a frame. Variables declared as frame parameters may be specified as parameters of the callframe, openframe, and gotoframe statements. For more information about these statements, see the Language Reference Guide online help.

Declare local variables that are not parameters and forward references to local procedures in the declare section of the initialize statement. If a declare block is not followed by a begin block of initialization statements, enddeclare is required at the end of the local variable declarations in the declare block.

You can use local variables declared in the initialize statement anywhere in the frame script or in any of the field or menu item scripts associated with the frame.

A local variable can be a simple variable of any acceptable base data type or a reference or array variable of any named user or system class. For more information about declaring variables, see "Language Elements" in the Language Reference Guide.

The local variables you declare with the initialize statement are in addition to the automatically declared variables that are associated with named fields and menu items. (You do not need to explicitly declare the variables associated with named fields and menu items.)

When you use the statement block, which is delimited by the keywords begin and end, OpenROAD executes the statements in this block when the frame is started, before displaying the frame on the window. You can use this statement block to perform any start-up operations, such as loading a table field or setting variables.

You may use the curly brackets { and } in place of the keywords begin and end.

## Frame Script Event Blocks

A frame script can include one or more event blocks. These blocks can contain frame events or events for any field or menu item defined for the frame.

An event block is a sequence of statements associated with one or more specific events. The syntax for an event block is:

```
on event [variablename]
    {, on event [variablename]} =
[declare
        localvariablelist
[enddeclare]]
begin
          statementlist
end[;]
```

When the event occurs, OpenROAD executes the statements specified by the statementlist.

### Rules for Coding Event Blocks

1. If the event is a field or menu item event, you must also specify the variable name associated with that field or menu item.

   Example:
   ```
   on click close_button =begin
        parent_frame.SendUserEvent
             (eventname = 'INDICATOR_OFF',
              messageinteger = row_number);
              return;
   end;
   ```

2. If the event is a frame event, do not include a variable name.

   Example:
   ```
   on childexit =
   begin
       statementlist
   end;
   ```

3. You can include more than one event type in an event block.

4. Declare local variables in the declare section of an event block.

5. Separate the statements in the statementlist with semicolons.

### Typical Frame Events

- Terminate: Exits the frame
- ChildEntry: Enters a subform, composite field, or any field on the form
- ChildExit: Exits a subform, composite field, or any field on the form
- ChildSetValue: Changes a subform, composite field, or any field on the form
- FrameResized: Changes the size of the window
- UserEvent: Specifies a user event

For more information about these events, see the Language Reference Guide online help.

## Examples—Frame Event Blocks

### Standard Quit Sequence

```
on click menu.file_menu.quit_menu,
on click quit_button =
begin
    return;
end;
```

### Standard Frame Termination Sequence

```
on windowclose,
on terminate =
begin
    if CurFrame.Topform.HasDataChanged = TRUE then
        /* save data */
    endif;
end;
```

### Typical Field on a Form

```
on childsetvalue =
begin
    data_is_changed = TRUE;
end;
```
