Rollback Statement
This statement rolls back the current database transaction.
This statement has the following syntax:
rollback [work ][to savepointname];
The rollback statement undoes part or all of the current transaction. If you omit the to savepointname clause, the statement terminates the transaction and rolls back any changes made by the transaction.
If you use the to savepointname clause, the transaction is not terminated, and only those changes made after the specified savepoint are rolled back. Processing resumes with the statement following the rollback statement. A rollback statement also closes any open cursors.
Note:  Do not use the to savepointname clause if there are open cursors in the transaction or if you are using a database procedure. Also, the optional work keyword is provided for compatibility with some versions of SQL.
You can use the rollback statement in a database procedure only if the procedure is directly executed and is not invoked by a rule.
If you are running in the autocommit on transaction management state, it is not possible to use the rollback statement, because the DBMS automatically commits each database statement when the statement completes. For more information about transaction management, see the Programming Guide.
The system class, DBSessionObject Class, when used with the RollbackWork method, is equivalent to this statement.
Parameters--Rollback Statement
This statement has the following parameters:
work
Is an optional keyword included for compatibility with the ISO and ANSI SQL standards
to savepointname
Rolls back only those changes made after the specified savepoint. The transaction is not terminated. Processing resumes with the statement following the rollback to savepointname statement. If autocommit is enabled, the rollback statement has no effect.
If rollback is issued without the optional to clause, the statement terminates the transaction and rolls back any changes made by the transaction.
Permissions: All Users
This statement is available to all users.
Rollback Locking
If the rollback statement is issued without the to savepoint_name option, the statement terminates the transaction and releases all locks held during the transaction. If the to savepoint_name option is included, no locks are released.
Rollback Performance
Executing a rollback undoes some or all of the work done by a transaction. The time required to do this is generally the same amount of time taken to perform the work.
Example--Rollback Statement
Back out a database statement:
insert into employees(salary, age)
     values(:salfield, :agefield);
if iierrornumber != 0 then
     message 'A bad thing happened.';
     rollback work;
endif;
/* Give the user a choice at close */
on terminate =
begin
     if CurFrame.ConfirmPopup(messagetext =
          'Commit database changes?') = PU_OK then
          commit work;
     else
          rollback work;
     endif;
end
