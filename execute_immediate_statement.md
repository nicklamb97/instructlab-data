Execute Immediate Statement
This statement executes an SQL statement.
This statement has the following syntax:
execute immediate statement_string
          [into variable{, variable}
          [beginendblock]];
The execute immediate statement performs any of the following functions:
•Executes SQL statements that you cannot include directly in your 4GL script
•Executes SQL statements constructed at runtime
•Includes new SQL functionality in your 4GL scripts
If you do not include a statement string, a runtime error occurs when the execute immediate statement is executed.
A select statement as the value of statement_string can return one or more rows. If it returns more than one row, the select statement can contain a select loop to process the rows. A select statement executed with the execute immediate statement cannot be a repeated select.
Use the into clause when the value of statement_string is a select statement. When the select statement executes, the values it retrieves are placed in the variables named in the into clause. The number and data types of these variables must match the number and data types of the values returned by the select statement. For more information, see Select Statement.
Parameters--Execute Immediate Statement
This statement has the following parameters:
statement_string
Specifies a string literal, a varchar expression, or a reference variable of class StringObject. The statement string cannot contain more than one statement. The varchar expression should be declared not null, or there will be compiler warnings.
It also cannot contain a Dynamic SQL statement or any of the following SQL statements:
•call
•close
•connect
•declare
•disconnect
•execute immediate
•fetch
•include
•inquire_sql
•open
•prepare to commit
•set_ingres
•whenever
Because the Value attribute of the StringObject will be passed to the command, it will be limited to 2000 characters. For more information, see the StringObject class Value Attribute).
variable
Specifies the variable that holds the value of the select statement. The number and data types of these variables must match the number and data types of the values returned by the select statement.
beginendblock
See BeginEndBlock.
Execute Immediate Description
The execute immediate statement executes a dynamically built statement string. This statement does not name or encode the statement and cannot supply parameters.
The execute immediate can be used:
•If a dynamic statement needs to be executed just once in your program
•In drop statements, where the name of the object to be dropped is not known at the time the program is compiled
The execute immediate statement must be terminated according to the rules of the host language. If the statement string is blank or empty, the DBMS Server returns a runtime syntax error.
The following SQL statements cannot be executed using execute immediate:
•call
•close
•connect
•declare
•disconnect
•enddata
•fetch
•get data
•get dbevent
•help
•include
•inquire_sql
•open
•prepare to commit
•put data
•set_sql
•whenever
•Other dynamic SQL statements
The statement string must not include references to variable names. If your statement string includes embedded quotes, it is easiest to specify the string in a 4GL language variable. If a string that includes quotes as a string constant is to be specified, remember that quoted characters within the statement string must follow the 4GL SQL string delimiting rules.
The into clause can only be used when the statement string is a select statement. The into clause specifies variables to store the values returned by a select. Use this option when the program knows the data types and lengths of the result columns before the select executes. The data type of the variables must be compatible with the associated result columns.
Permissions: All Users
This statement is available to all users.
Execute Immediate Locking
The locking behavior of the execute immediate statement depends on which statement is executed.
Examples--Execute Immediate Statement
Create a table defined at runtime by the user:
/* User input assigned to variable new_table */
new_table = 'Create table expired_accts(acctno =
          integer, name = varchar(50) not null)';
...
execute immediate new_table;
Execute a statement in a string object:
/* Declare the string object variable */
stmt_string = StringObject;

/* Prompt user for input */
status = CurFrame.ReplyPopup
          (messagetext = 'Enter an SQL statement',
                    reply = stmt_string):
/* Execute the statement */
execute immediate stmt_string;
Execute a select statement:
/* Declarations */
emp_no = integer not null;
emp_name = varchar(256) not null;
selstring = varchar(256) not null;
/* Executing code */
selstring = 'select e_no, e_name from emp_table';
execute immediate selstring into emp_no, emp_name
begin
     processing statements
end
