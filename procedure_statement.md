
# Procedure Statement

This statement defines a 4GL procedure.

This statement has the following syntax:

```
procedure procname [(parameterlist)] = [declareblock] beginendblock[;]
```

The procedure statement defines either a global or local 4GL procedure. A global 4GL procedure can define local procedures but a local procedure cannot.

For more information about using procedures, see the *Programming Guide*.

## Parameters--Procedure Statement

This statement has the following parameters:

**procname**

Specifies the name of the procedure. For a global procedure, this name must be the one that you specified when you coded the procedure in OpenROAD Workbench.

For a local procedure, this name must match the name that was specified as *localproceduredeclaration* in a declare block for a global procedure or in an initialize statement for a frame script, field script, or user class script.

**parameterlist**

See NamedParameterListDefinition.

**declareblock**

For a local procedure, see DeclareBlock.
For a global procedure, see DeclareBlockWithProcedures.

**beginendblock**

See BeginEndBlock.

## Examples--Procedure Statement

Define a procedure that adds the appropriate tax to a given money value:

```
procedure addtax (
    tax = float,
    amount = money
) =
begin
    return amount + amount*tax;
end
```

Define a procedure that displays a message (no variables needed):

```
procedure printmsg = 
begin
    message 'Here is a message';
end
```
