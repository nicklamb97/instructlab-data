Relocate Statement
The relocate statement is used to relocate tables. This statement moves a table from its current location to the area corresponding to the specified location_name. All indexes, views, and protections for the table remain in force and valid regardless of the location of the table.
Note:  The relocate statement must be used when the current disk of the table becomes too full.
This statement has the following syntax:
relocate table_name to location_name;
Parameters--Relocate Statement
This statement has the following parameters:
table_name
Specifies the name of the table to be relocated
location_name
Refers to the area in which the new table is created. The location name must be defined on the system, and the database must have been extended to the corresponding area.
Note:  Table_name and location_name must be string constants if this statement is used in an embedded program. Host language variables cannot be used to represent either.
Examples--Relocate Statement
The following example relocates the employee table to the area defined by the remote_loc location_name:
relocate employee to remote_loc;
