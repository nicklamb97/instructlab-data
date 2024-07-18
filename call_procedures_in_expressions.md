# How You Can Call Procedures in Expressions

If a procedure returns a value, you can use the procedure as an expression or as part of an expression in an OpenROAD statement. The data type of the return value must be compatible with the requirements of the expression.

To call a procedure as part of an expression, do not use the callproc keyword. Simply give the procedure name followed by the parameter list.

## Example

In the following example from the check_out frame script, the expression in the if statement calls the error_handler procedure:

```sql
if error_handler(frm = CurFrame, commit_evt =
        'Commit') != ER_OK then
    resume;
endif;
```

When you call a procedure in an expression, you must include the parentheses even if you are not passing parameters to the procedure. In the following example, the parentheses are empty because no parameters are being passed to the dbms_error_message procedure:

```sql
message 'Cannot insert graphic into the
    database. ' +dbms_error_message();
```

In OpenROAD, 4GL procedures can return a value of any type. You can operate on the return value with any operation appropriate to the return value type. In the previous example, the dbms_error_message procedure returns a text value. Therefore you can use the return value within the text expression.

If the return value is a reference variable, you can manipulate it as any other reference variable. For example, you can use dot notation to access a specific attribute or to invoke a method of the object.
