# Message Statement

This statement displays a message (character string) on the screen in a pop-up window.

This statement has the following syntax:

```
message varcharstringexpr;
```
The pop-up window is centered in the frame that executes the statement. The pop-up covers the calling frame and remains on display until the user clicks its OK button. The calling frame is inactive while the pop-up is displayed.

**Note:** For another way to display a simple message, see the ProcExec class InfoPopup Method.

## Parameters--Message Statement

This statement has the following parameter:

**varcharstringexpr**

Specifies a character string containing the message to be displayed. Enclose this string in quotes if it is a string literal.

## Example--Message Statement

Display an error message:
```
message 'Bad Value';
```
