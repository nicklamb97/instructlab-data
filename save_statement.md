Save Statement
The save statement directs the DBMS Server to save the specified table until the given expiration date. By default, base tables have no expiration date. An expiration date cannot be assigned to a system table.
This statement has the following syntax:
save [schema.]table_name [until month day year];
Parameters--Save Statement
This statement has the following parameters:
table_name
Specifies the name of the table to be saved
month
Must be specified as an integer from 1 through 12, or the name of the month, abbreviated or spelled out.
day
Must be a valid day of the month (1 to 31), and year
year
Must be a fully specified year, for example, 2012.
The range of valid dates is January 1, 1970 through December 31, 2035, inclusive.
Note:  If the until clause is omitted, the expiration date is set to no expiration date. To purge expired tables from the database, use the verifydb command. Expired tables are not automatically purged.
Permissions: Own the table
You must own the table.
Save Locking
The save statement takes an exclusive lock on the specified table.
Examples--Save Statement
The following example saves the employee table until the end of February 2011:
save employee until feb 27 2011;
