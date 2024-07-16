# Set_4gl Statement
This statement sets error display status.
This statement has the following syntax:

```4gl
exec 4gl set_4gl(messages = value);
```

The set_4gl statement turns error reporting to the screen on and off. By default, errors from EXEC 4GL statements are displayed.
This statement has the following parameter:

### value
- Set to 1 to display error messages from EXEC 4GL statements, 0 to suppress them.
- This parameter is an integer.