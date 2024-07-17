Inquire_sql Statement
This statement provides runtime status information about the most recently executed SQL statement.
This statement has the following syntax:
inquire_sql(variablename = inquire_sql_constant
          {, variablename = inquire_sql_constant});
The inquire_sql statement retrieves runtime status information about the most recently executed SQL statement. You can obtain the following information:
•Local and generic error numbers
•Error message text
•The text and number of a message statement in a database procedure
•The number of rows affected by the SQL statement
•Whether a transaction is open
Parameters--Inquire_sql Statement
This statement has the following parameters:
variablename
Specifies a field or local variable that was declared in the procedure or frame issuing the inquire_sql statement. The variable's data type must be compatible with the data type of the information that you are assigning to it.
The field or variable is used to hold information retrieved in the inquire_sql statement.
inquire_sql_constant
Specifies a constant that determines what information the statement returns. You can use any of the constants listed in the following table:
Constant
Data Type
Description
dbmserror
integer
The error number that the last query produced. This is the same number that is in system variable IIDBMSerror.
endquery
integer
If the previous fetch statement found no more rows to return, endquery returns the value one. If the last fetch statement returned a valid row, the value returned is zero.
errorno
integer
A positive integer, representing the error number of the last query. The error number is cleared before each SQL statement, so that this object is only valid immediately after the statement in question. This is the same number that is in IIerrornumber.
If a statement executes with no error, then error number is set to zero.
errortext
varchar
The error text of the last query containing the complete error message of the last error. The error text is only valid immediately after the database statement in question.
The errortext constant returns the text of the local error number. The length of the variable to which you assign errortext determines the length of the message returned. The message includes the error number and a trailing newline character. The message is blank if no error occurred.
You can use the DBMSErrorPrinting attribute of the SessionObject system class to output this information to a window or a printer.
errortype
varchar
Returns genericerror if Ingres returns the generic error number to errorno, or dbmserror if OpenROAD returns the local DBMS error number to errorno.
messagenumber
integer
The number of the last SQL message statement executed inside a database procedure. If there was no message statement, messagenumber returns the value zero. The message number is defined by the database procedure programmer.
messagetext
varchar(2000)
Returns the message text of the last SQL message statement executed inside a database procedure. If the message statement had no text, this constant returns a blank. OpenROAD truncates the text if the receiving variable is shorter than the text.
prefetchrows
integer
Returns the number of rows buffered when fetching data using cursors. This value is reset every time a cursor is opened.
rowcount
integer
Returns the total number of rows affected by the last SQL statement, that is, subject to any of the following statements: insert, delete, update, select, fetch, modify, create index, create table as, select, or copy.
The value of rowcount is the same value as IIrowcount.
session
integer
Returns the session identifier of the current database session. If the application is not using multiple sessions or there is no current session, session returns the value zero.
transaction
integer
Returns a value of one if a transaction is open, or zero if no transaction is open.
Example--Inquire_sql Statement
Check for an error before committing a database statement:
insert into newemp(name, address, town)
     values('Jones', '143 Longview Dr', 'Anytown');
inquire_sql(mistake = errorno);
if mistake = 0 then
     commit;
else
...
