Savepoint Statement
The savepoint statement declares a named savepoint marker within a transaction. Savepoints can be used in conjunction with the rollback statement (see Rollback Statement) to roll back a transaction to the specified savepoint when necessary. Using savepoints can eliminate the need to roll back an entire transaction if it is not necessary.
Note:  This statement has additional considerations when used in a distributed environment. For more information, see the Ingres Star User Guide.
This statement has the following syntax:
savepoint savepoint_name;
Parameters--Savepoint Statement
This statement has the following parameter:
savepoint_name
Defines the name of the savepoint. The name can be any unquoted character string conforming to rules for object names, except that the first character need not be alphabetic. This enables numeric savepoint names to be specified.
Any number of savepoints can be declared within a transaction, and the same savepoint_name can be used more than once. However, if the transaction is aborted to a savepoint whose name is used more than once, the transaction is backed out to the most recent use of the savepoint_name.
All savepoints of a transaction are rendered inactive when the transaction is terminated (with either a commit, a rollback, or a system intervention upon deadlock). For more information on deadlock, see Rollback Statement, and Commit, and the chapter “Working with Transactions and Handling Errors” in the Ingres SQL Reference Guide.
Permissions: All Users
This statement is available to all users.
Savepoint Related Statements
Rollback Statement
Examples--Savepoint Statement
The following example declares savepoints among other SQL statements:
insert into emp (name, sal, bdate)
        values ('Jones,Bill', 10000, 1945);
/*set first savepoint marker */
savepoint setone;
insert into emp (name, sal, bdate)
        values ('Smith,Stan', 20000, 1948);
/* set second savepoint marker */
savepoint 2;
insert into emp (name, sal, bdate)
        values ('Engel,Harry', 18000, 1954);
/* undo third append; first and second remain */
rollback to 2;
/* undoes second append; first remains */
rollback to setone;
commit;
/* only the first append is committed */
