Remove Table Statement
The remove table statement removes the registration of a security log file.
This statement has the following syntax:
remove table [schema.]table_name;
The remove table statement removes the mapping of a file to a virtual table. To map files to virtual tables, use the register table statement (see Register Table Statement). The remove table statement removes security log files that were registered using the register table...as import statement.
Note:  This statement is not the same as the remove statement, which is described in the Ingres Star User Guide.
Parameters--Remove Table Statement
This statement has the following parameter:
table_name
Specifies the name of the table
Remove Table Permissions
You must have SECURITY privilege to remove a table. However, if the target table being removed is a security audit gateway table (that is, was registered with dbms=SXA), you must have MAINTAIN_AUDIT privilege.
Remove Table Locking
The remove table statement locks the iirelation, iiattribute, iiqrytext, and iiregistrations catalogs.
Remove Table Related Statements
Register Table Statement
Examples--Remove Table Statement
The following example removes a security log registration:
remove table logfile_xyz;
