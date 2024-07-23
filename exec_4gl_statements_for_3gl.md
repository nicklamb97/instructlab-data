# EXEC 4GL Statements for 3GL

In OpenROAD you can use EXEC 4GL statements in an Embedded SQL for C (ESQL/C) program to pass objects and arrays between 3GL and 4GL routines.

The syntax for an EXEC 4GL statement is:

**exec 4gl** *4gl_statement* [*terminator*]

***4gl_statement***
Specifies one of the 4GL statements described in the sections that follow

***terminator***
Specifies a termination character for the statement, typically a semi-colon (;)

**Note:**  You must always place the EXEC 4GL keywords before each statement. This syntax is similar to that for Embedded SQL and SQL/Forms.

These statements can be used only in Embedded SQL programs that pass objects and arrays between 3GL and 4GL routines.

Structured data is passed to 3GL as a handle. You reference the handle in the EXEC 4GL statements whenever you want to indicate the object or array. This handle is always a 4-byte integer.

For a discussion about using handles and passing objects and arrays between 4GL and 3GL routines, see the *Programming Guide*.
