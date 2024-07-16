Close Statement
This statement closes an open cursor.
This statement has the following syntax:
close cursor_variable;
The close statement closes a cursor and sets the State attribute of the CursorObject to CS_CLOSED. When the cursor is closed, you cannot use it for further processing unless you reopen it with a new open statement (see Open Statement). To use the close statement, you must have previously opened the specified cursor object with an open statement.
The commit and rollback statements implicitly close all open cursors. For more information about using cursors, see the Programming Guide.
Parameters--Close Statement
This statement has the following parameter:
cursor_variable
Is a reference variable that points to an object of the CursorObject class
Example--Close Statement
Close an open cursor:
close emp_cursor;
