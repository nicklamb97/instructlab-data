Open Statement
This statement opens a cursor in the database, evaluates the specified statement, and associates the select statement's result table with the cursor.
This statement has the following syntax:
open cursor_variable for select_statement
          [for update_clause];
where update_clause is one of the following:
readonly [deferred | direct] update of column{, column}
The open statement performs the following actions:
•Opens a cursor in the database
•Evaluates the specified select statement and positions the cursor immediately before the first row in the select statement's result table
•Sets the State attribute to CS_OPEN
You must use the open statement before you can use any data manipulation statements (fetch, update, or delete) with the cursor. After the cursor is open, it stays open until you close it with the close statement or until the transaction terminates.
The select statement retrieves a set of rows from the database. These rows are then associated with the cursor. (The set of rows is called a result table.) The target list of the select statement can use any of the following formats:
resultname = dbexpression
dbexpression as resultname
*
correlation_name.*
dbexpression
Note:  The asterisk selects values from all columns.
See Select Statement for a complete description of its syntax. If you intend to specify the "for update" clause (described in the next section) in your open statement, its associated select statement can reference only one table and cannot contain a distinct, group by, having, order by, or union clause.
Parameters--Open Statement
This statement has the following parameters:
cursor_variable
Specifies the name of a local or global reference variable that points to an object of type CursorObject. This variable cannot have a null value.
select_statement
Specifies any legal select statement. If you open the cursor for update, its associated select statement can reference only one table.
update_clause
Defines the operations that you intend to perform on the rows retrieved by the select statement. This clause can have any of the following formats:
readonly [deferred | direct] update of column{, column}
column
Specifies the name of a column in the table that you intend to update. You cannot update a column that is not included in this list.
How You Can Define the Cursor Mode
The for update_clause defines the cursor mode. You can open a cursor in either of the following modes:
Read-only
Specifies that you can read the result table rows but not update or delete them
Update
Specifies that you can read, update, or delete rows in the result table
If you omit this clause, you can read or delete the result table rows but you cannot update them.
You can use the deferred and direct keywords to specify when updates and deletions are visible in the database. If you specify a deferred update cursor, then any updates or deletions made are not visible in the database until the cursor is closed. If you specify a direct update cursor, then any changes or deletions are visible immediately. If you do not specify one of these keywords, the default is deferred update.
If you are running under the default transaction management state (autocommit is off), you can open one deferred cursor and any number of direct update cursors and any number of read‑only cursors. If you are running with autocommit on, then you can have only one cursor open at any one time, regardless of its mode.
For more information about autocommit and transaction management, see the Programming Guide.
For more information about using cursors in OpenROAD applications, see Fetch Statement and the Programming Guide.
Examples--Open Statement
The following example opens emp_cursor as a read-only cursor, using the current value of the dept# variable in the where clause of the select statement:
open emp_cursor for select name, age
     from emp
     where dept = :dept#
     for readonly;
The following example opens emp_cursor so that the user can update the age column:
open emp_cursor for select name, age
     from emp
     where dept = :dept#
     for direct update of age;
