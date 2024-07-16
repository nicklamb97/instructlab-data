# The `prompt` statement in Actian 4GL

The prompt statement in Actian OpenROAD 6.2 displays a message in a pop-up window and accepts input from the application user.

## Syntax
```4gl
variable = prompt varcharstringexpr;
```

## Description
The prompt statement causes the application to request input from the user. It displays a character string message on the screen, accepts the input from the user, and places the user's response into a designated variable.
- The prompt is displayed in a pop-up window centered in the calling frame.
- This pop-up window covers the calling frame until the user acknowledges the prompt.

## Parameters
### variable
Specifies the name of a varchar variable to which the user's response is assigned. If the response is longer than the variable, OpenROAD truncates the response on the right.

### varcharstringexpr
Specifies the message to be displayed. This message must be a varchar expression with a maximum length of 2000 characters.

## Examples
### Example 1: Prompt for a Report Name
Prompt for a report name and place the response in the answer variable:
```4gl
answer = prompt 'Enter the report name: ';
```

## Example 2: Prompt for Department
Prompt the user for the department of the person whose name appears in the fname and lname fields:
```4gl
answer = prompt 'Enter the department for ' + fname + ' ' + lname;
```
