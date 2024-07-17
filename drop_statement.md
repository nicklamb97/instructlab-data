Drop Statement
The drop statement destroys one or more tables, indexes, or views.
Note:  This statement has additional considerations when used in a distributed environment. For more information, see the Ingres Star User Guide.
Note:  This statement has the following syntax:
drop [object_type] [schema.]object_name {, [schema.]object_name};
Parameters--Drop Statement
This statement has the following parameters:
object_type
Specifies the type of object to drop, which can be one of the following keywords:
table
view
index
The following object types can also be dropped. These drop statements are described under separate entries in this chapter:
dbevent (see Drop Dbevent Statement)
group (see Drop Group Statement)
integrity (see Drop Integrity Statement)
link (applies to Ingres Star only--see the Ingres Star User Guide)
procedure
role (see Drop Role Statement)
rule (see Drop Rule Statement)
object_name
Specifies the name of a table, view, index, or other valid object.
Drop Description
The drop statement removes the specified tables, indexes, or views from the database. Any synonyms and comments defined for the specified object are also dropped. If the object is a table, any indexes, views, privileges, and integrities defined on that table are automatically dropped.
If an object type is not specified, Ingres assumes you are dropping a table, view, or index.
If the keyword indicating the object type is specified, the DBMS Server checks to make sure that the object named is the specified type. If more than one object is listed, only objects of the specified type are dropped. For example, if employee is a base table and emp_sal is a view on the base table salary, the following statement:
drop table employee, emp_sal;
drops only the employee base table (because the keyword table was specified and emp_sal is a view, not a base table).
To drop a combination of table, views, and indexes in a single statement, omit the object_type keyword. For example:
drop employee, emp_sal;
If an object that is used in the definition of a database procedure is dropped, all permits on the procedure are dropped (the procedure is not dropped). The procedure cannot be executed, nor can the execute privilege be granted on the procedure until all the objects required by its definition exist.
All temporary tables are deleted automatically at the end of the session.
Drop Permissions
You must be the owner of a table, view, or index.
Drop Locking
The drop statement takes an exclusive lock on the specified table.
Drop Related Statements
Create Table Statement
Create View Statement
Examples--Drop Statement
The following are drop statement examples:
1.Drop the employee and dept tables.
drop table employee, dept;
2.Drop the salary table and its index, salidx, and the view, emp_sal.
drop salary, salidx,
accounting.emp_sal;
3.In an OpenROAD program, drop two views.
drop view tempview1, tempview2;
