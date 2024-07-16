Delete Statement
This statement deletes rows from a database table.
This statement has the following syntax:
•Non-cursor version:
[repeated] delete from tablename [corrname]
     [where searchcondition];
•Cursor version:
delete from tablename where current of cursor_variable;
The non-cursor version of the delete statement deletes selected rows from a table. The cursor version deletes one row at a time.
Non-cursor Version
The where clause in the non-cursor version lets you select the rows you want to delete. If you omit the where clause, the statement deletes all of the rows in the table.
The repeated keyword tells OpenROAD to encode and save the query execution plan for the statement. Use this method as a performance enhancement if you intend to run the query more than once in your program.
Note:  This keyword is supported for Ingres database systems only. If other DBMS systems are being used, it is ignored.
The saved query execution plan is based on the initial values found in all parameters of the syntax. Therefore, do not use the repeated option if the where clause searchcondition is represented by a single variable or if your table name is a dynamic name.
Cursor Version
The cursor version of the delete statement deletes the row to which the specified cursor is pointing. The cursor is specified in the "where current of" clause. The value of cursor_variable must be the name of a reference variable that points to an object of the CursorObject class. The table name that you specify in a cursor delete statement must match the name of the table that you specified in the cursor's open statement.
Before you can issue a delete statement for a cursor, you must first open the cursor and fetch a row. If you open the cursor for direct update, any deletions take effect immediately. If you open the cursor for deferred update, any deletions take effect when you close the cursor.
You must execute a cursor delete statement in the same DBMS session in which you opened the cursor.
A successful cursor delete statement sets the State attribute of the CursorObject to CS_NOCURRENT, indicating that the cursor is now inting to a position after the deleted row but before the next row. You must issue another fetch statement before issuing another delete statement (or an update statement). A successful delete cursor statement also sets the IIrowcount system variable to one.
Parameters--Delete Statement
This statement has the following parameters:
tablename
Specifies the name of the table from which the row is deleted. It is used in both versions of the delete statement. This is a dynamic name.
corrname
Specifies the correlation name of the table. It is used in only the non-cursor version.
searchcondition
Specifies a logical expression of conditions that must be satisfied by all rows selected. You can use simple variables in a search condition wherever you can use a constant. Alternatively, you can place the entire search condition in a single varchar variable.
cursor_variable
Specifies the reference variable that points to the CursorObject for which the statement is issued. It is used only in the cursor version.
Examples--Delete Statement
Delete all rows in the personnel table containing the employee number (empno) displayed in the current frame, using the repeated option, then commit the changes:
repeated delete from personnel
          where personnel.empno = :empno;
commit;
Delete all rows in the personnel table:
delete from personnel;
Delete all rows in the table specified in the tablename field:
delete from :tablename;
Delete rows from a table, using variables in the statement to represent the table name and the where clause search condition:
whereclause = 'empno = 12';
tablename = 'personnel';
delete from :tablename where :whereclause;
Delete the row pointed to by the emp_cursor object:
delete from emp where current of emp_cursor;
if iirowcount = 0 then
message 'Delete failed';
endif;
