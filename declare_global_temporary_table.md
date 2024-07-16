Declare Global Temporary Table Statement
This statement creates a temporary table.
This statement has the following syntax:
declare global temporary table session.table_name
          (column_name format{, column_name format})
          [withclause];
To create a temporary table by selecting data from another table:
declare global temporary table session.table_name
          (column_name{, column_name})
          as subselect
          [withclause];
Valid parameters for the with clause are:
location = (locationname{, locationname})
[no]duplicates
allocation=initial_pages_to_allocate
extend=number_of_pages_to_extend
For temporary tables created using a subselect, the following additional parameters can be specified in the with clause:
structure = hash | heap | isam | btree
key = (columnlist)
fillfactor = n
minpages = n
maxpages = n
leaffill = n
nonleaffill = n
compression[ = ([[no]key][,[no]data])] | nocompression
page_size = n
You must specify multiple with clause parameters as a comma-separated list. For details about these parameters, see the statement description for create table, in this chapter. To delete a temporary table, use the drop table statement.
The session table owner is required for the declare global temporary table session statement; you cannot omit these keywords.
The declare global temporary table statement creates a temporary table, also referred to as a session-scope table. Temporary tables are useful in applications that need to manipulate intermediate results and want to minimize the processing overhead associated with creating tables.
Temporary tables reduce overhead in the following ways:
•No logging is performed on temporary tables.
•No page locking is performed on temporary tables.
•Disk space requirements are minimized. If possible, the temporary table is created in memory and never written to disk.
•No system catalog entries are made for temporary tables.
•Temporary tables have the following characteristics:
–They are visible only to the session that created them.
–They are deleted when the session ends (unless deleted explicitly by the session). They do not persist beyond the duration of the session.
–They can be created, deleted, and modified during an online checkpoint.
If you omit the location parameter, the temporary table will be located in the default database location (if the temporary table requires disk space). If you omit the subselect, the temporary table is created as a heap.
Temporary tables are assigned the schema “session”; you cannot create two temporary tables with the same name, but can assign the name of an existing table to a temporary table without conflict. For example, a user named joe, who owns a table named employees, can create a temporary table with the same name. The DBMS resolves the permanent table's name as joe.employees and the temporary table's name as session.employees. To prevent names of temporary tables from conflicting with names of permanent tables, you must specify all references to the temporary table as session.table_name.
To delete a temporary table, issue the drop table session.table_name statement.
When a transaction is rolled back, any temporary table that was in the process of being updated will be dropped (because the normal logging and recovery processes are not used for temporary tables).
Other guidelines include:
•You can use host language variables to specify constant expressions in the subselect of a create table...as statement.
•You can specify locationname using a host language string variable.
•The preprocessor does not validate the syntax of the withclause.
•Do not specify the declare global temporary table session statement within the declare section of the OpenROAD program; place the statement in the body of the OpenROAD program.
Restrictions on Temporary Tables
Temporary tables are subject to the following restrictions:
•Temporary tables cannot be used within database procedures.
•Temporary tables cannot be used in view definitions.
•You cannot create integrities, constraints, or user-defined defaults for temporary tables. (You can specify with | not null and with | not default.)
•The following SQL statements cannot be used on temporary tables:
–create index
–create permit
–create synonym
–create view
–grant
–revoke
–save
–set journaling
–set lockmode
–create security_alarm
–help
–create integrity
–create rule
All other SQL statements can be used with temporary tables.
•Repeat queries referencing temporary tables cannot be shared between sessions. Any session can create temporary tables and no locking is performed.
For more information, see the following statement descriptions:
•Delete Statement
•Insert Statement
•Select Statement
•Update Statement
Examples--Declare Global Temporary Table Statement
To create a temporary table:
declare global temporary table
          session.emps
          (name char(20), empno char(5))
          with location = (personnel),
          [no]duplicates,
          allocation=100,
          extend=100;
To use a subselect to create a temporary table containing the names and employee numbers of the highest-rated employees.
declare global temporary table
          session.emps_to_promote
          as select name, empno from employees
          where rating >= 9
