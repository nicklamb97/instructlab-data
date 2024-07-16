Commit Statement
This statement commits the current database transaction.
This statement has the following syntax:
commit [work];
Note:  The optional work keyword is used only for compatibility with some versions of SQL.
The commit statement terminates the current database transaction and commits any changes made by the transaction. After you issue a commit statement, you cannot undo (roll back) any changes to the database made by the committed transaction. A commit statement also closes any open cursors.
If the application terminates while a transaction is open, the database issues an implicit commit before terminating the application.
It is not necessary to issue explicit commit statements if you are running the application with autocommit on. When autocommit is on, the database automatically commits each database statement when it completes successfully. The default for autocommit is off.
For more information about autocommit and transaction management, see the Programming Guide.
If the commit statement is successful, it sets the system variable IIerrornumber to zero. Otherwise, it sets IIerrornumber to the appropriate error number. In both cases, IIrowcount is set to -1. For more information about these system variables, see the Programming Guide.
The system class, DBSessionObject, when used with the CommitWork method is equivalent to this statement. For more information about this system class and method, see DBSessionObject Class.
Examples--Commit Statement
Commit a database statement:
insert into employees(salary, age)
values(:salfield, :agefield);
commit work;
The following event block gives the user a choice to commit or roll back changes before the frame closes:
on terminate =
begin
if CurFrame.ConfirmPopup(messagetext =
'Commit database changes?') = PU_OK then
commit work;
else
rollback work;
endif;
end
