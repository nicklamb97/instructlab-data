Execute Procedure Statement
This statement executes a database procedure.
This statement has the following syntax:
[variable =] execute procedure procname
          ([parameterlist])
          [into cursorobjreference{, cursorobjreference}];
The execute procedure statement executes a database procedure. This procedure must be registered before the execute procedure statement calls it, although registration does not need to be done within the application that issues the execute procedure statement.
The execute procedure statement lets you pass simple variable values to the procedure using the optional parameter list. The data type of the value that you assign to a parameter must be compatible with the data type of the parameter.
The procedure can return a single value to the procedure or frame that executed the execute procedure statement. The variable that receives the return value must be an integer data type.
Using the optional into cursorobjreference clause allows database procedures to return data sets. This optional clause is currently not available running against the Ingres DBMS.
If you are using the execute procedure statement with non-Ingres servers, see the section "Database Procedure Management" in the Enterprise Access Developing Portable Applications Guide for information about declaring and executing DBMS procedures (see http://docs.actian.com/).
Parameters--Execute Procedure Statement
This statement has the following parameters:
variable
Contains the return value of the procedure
procname
Specifies the name of the procedure as specified in the procedure's definition
parameterlist
See NamedParameterList.
cursorobjereference
(Optional) Allows database procedures to return data sets. This clause is currently not available running against the Ingres DBMS.
Execute Procedure Permissions
To execute a procedure that you do not own, you must have EXECUTE privilege for the procedure, and must specify the schema parameter.
Execute Procedure Locking
The locks taken by the procedure depend on the statements that are executed inside the procedure. All locks are taken immediately when the procedure is executed.
Execute Procedure Performance
The first execution of the database procedure can take slightly longer than subsequent executions. For the first execution, the host DBMS may need to create a query execution plan.
Execute Procedure Related Statements
Grant (privilege) Statement
Examples--Execute Procedure Statement
Assume the following database procedure definition:
create procedure myproc
          (emp_name = varchar(256) not null, emp_no = integer not null,
           emp_terminated = integer not null) =
begin
          processing statements
end
Then execute the procedure myproc:
execute procedure myproc(emp_name = 'joshw',
          emp_no = 12, emp_terminated = byref(EmpIsTerminated));
Example--Exit Statement
The following event block, defined for the Quit menu operation, uses an exit statement to end the application:
on click file.quit =
begin
          exit;
end
