# The `resume` statement in Actian 4GL

The resume statement in Actian OpenROAD 6.2 ends the currently executing event block and can be used in different ways to control the flow of events.

## Syntax
```4gl
resume [next];
```

## Usage
### Without the next Keyword
- Ends the current operation.
- Repositions the input focus on the screen at the field designated in CurFrame.InputFocusField.
### With the next Keyword
- Gives control to the next pending event that was triggered by the original action in the event block.

## Behavior
### Omitting the next Keyword
- The field designated in the InputFocusField attribute is the field that last had the input focus when the event block was activated.
- You can force control to resume to a specific field by explicitly setting the InputFocusField attribute of the FrameExec object to the desired field.
### Handling Multiple Events
- User actions can trigger more than one event. If the next keyword is not used, these events are discarded.
- Example: If a user tabs between two entry fields, A and B, with an Exit event for A and an Entry event for B, a resume statement in the Exit event block will position the cursor on field A and discard the Entry event for field B. A resume next statement will position the cursor on field B and process the Entry event for B.
 
## Event Chains and Event Processing
For detailed information on event chains and event processing, refer to the Programming Guide.

### Database and User Events
- A resume statement never discards pending database events and usually does not discard user events.
- Exception: If a SendUserEvent call sets the focus behavior of the current field in the receiving frame to FT_SETVALUE or FT_TAKEFOCUS, and a preceding event block executes a resume statement (without next), the rest of the chain, including the user event, is discarded.

## Placement of the resume Statement
- Typically the last statement in an event block or in the statement list of an if statement.
- Statements following a resume statement are not executed.
- Often used in a SetValue validation if the condition fails.

## Use in Procedures
- Can be used in local or global procedures.
- Terminates the currently executing event block and all procedures above it in the call stack until a frame is found.
- If no currently executing event block exists, the application is terminated.

## Examples
### Example 1: Resume Control Based on Field Value
Assume you have four fields on a form: testfield, field1, field2, and field3:

```4gl
on exit testfield =
begin
    if testfield = 2 then
        CurFrame.InputFocusField = field(field2);
        resume;
    elseif testfield = 3 then
        CurFrame.InputFocusField = field(field3);
        resume;
    endif;
    /* processing statements for field1 */
end
```
- Control resumes on field2 if testfield is 2, and on field3 if testfield is 3.
- For any other value, control resumes on field1 if the user tries to tab out of the field.

## Example 2: Validate Field and Reposition Input Focus
Reposition input focus on the vendor_num field with an error message if the current field contents are not valid:

```4gl
on setvalue vendor_num =
begin
    select vendornum as tvnum
        from vendor
        where vendornum = :vendor_num;
    if iirowcount = 0 then
        message 'Vendor does not exist.';
        resume;
    endif;
end
```
- If the vendor_num does not exist in the vendor table, an error message is displayed and control is resumed on the vendor_num field.
