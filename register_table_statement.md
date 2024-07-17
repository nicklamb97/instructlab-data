Register Table Statement
The register table statement maps the structure of a file to the structure of a table.
This statement has the following syntax:
register table [schema.]table_name
              (column_name column_type [is 'external_name']
              {, column_name column_type [is 'external_name']})
              as import | link from 'security_log_file_name' | 'current'
              with dbms = SXA [, rows = integer_value];
Parameters--Register Table Statement
This statement has the following parameters:
table_name
Assigns a name to the table.
column_name column_type [is 'external_name']
Specifies the name and data type of each column of the virtual table.
The is 'external_name' clause maps the columns in the virtual table to the fields in the file (external_name). For example, the following statement maps the table column, db_name, to the security log field, database:
db_name char(32) is 'database'
If the is clause is omitted, the column names must correspond to the field names listed in the file. At least one column must be specified. Columns can be specified in any order.
as import from
Specifies the file whose contents are to be imported. Valid values are:
'current'
Dynamically registers the current log file that is in use. If 'current' is specified, SQL operations on the virtual log table always see the log file in use, even if the physical log file changes.
'security_log_file_name'
Specifies the name of the security log file. The name must be specified as a quoted string, and must be a valid operating system file specification.
as link from
Registers existing local database tables, views, and database procedures in a distributed database.
with
Specifies additional information about the table being registered.
dbms=
Specifies the origin of the table being registered.
To register a security log, specify SXA.
By default, the security log shows security events for the entire Ingres installation. If the database field is omitted, the security log contains records only for the database in which the log is registered.
rows=integer_value
Specifies the number of records the log is expected to contain; the default is 1000. This value is displayed by the help table statement as Rows: and is used by the DBMS query optimizer to produce query plans for queries that see the registered table.
Register Table Description
The register table statement maps the fields in a file to columns in a virtual table. After registering the file as a table, use SQL to manipulate the contents of the file. The registered table can be referred to in database procedures. To delete a registration, use the remove table statement (see Remove Table Statement).
Note:  
•For information about registering IMA tables, see the Ingres System Administrator Guide.
•This statement is not the same as the register...as link statement, which is described in the Ingres Star User Guide.
The following statements can be performed against registered tables:
•create view
•create synonym
•create rule
•comment
•select
•insert, update, and delete (if they are from an updatable Enterprise Access product)
•drop
•save
•register...as link (as described in the Ingres Star User Guide)
The following statements cannot be performed against registered tables:
•modify
•create index
Security Log File Format
The security log is created and written when security logging is enabled (using the enable security_audit statement). The security log file has the following format:
Field Name
Data Type
Description
audittime
date
Date and time of the audit event
user_name
char(32)
Effective user name
real_name
char(32)
Real user name
userprivileges
char(32)
User privileges
objprivileges
char(32)
Object privileges
database
char(32)
Database
auditstatus
char(1)
Status of event; Y for success or N for failure
auditevent
char(24)
Type of event
objecttype
char(24)
Type of object
objectname
char(32)
Name of object
description
char(80)
Text description of event
objectowner
char(32)
Owner of the object being audited
detailnum
Integer(4)
Detail number
detailinfo
varchar(256)
Detail textual information
sessionid
char(16)
Session identifier
querytext_ sequence
integer(4)
Sequence number for query text records, where applicable
Note:  When registered, a security log is read-only.
Register Table Permissions
The session must have MAINTAIN_AUDIT privilege.
To query the audit log, AUDITOR privilege is required.
Register Table Locking
The register table statement locks pages in the iiregistrations, iirelation, iiattributes, and iiaudittables catalogs.
Related Statements
Remove Table Statement
Examples--Register Table Statement
The following example registers a security audit log with various attributes:
register table aud1 (
    audittime          date not null,
    user_name          char(32) not null,
    real_name          char(32) not null,
    userprivileges     char(32) not null,
    objprivileges      char(32) not null,
    database           char(32) not null,
    auditstatus        char(1) not null,
    auditevent         char(24) not null,
    objecttype         char(24) not null,
    objectname         char(32) not null,
    objectowner        char(32) not null,
    description        char(80) not null,
    detailinfo         varchar(256) not null,
    detailnum          integer4 not null,
    sessionid          char(16) not null,
    querytext_sequence integer4 not null
) as import from 'myfile'
with dbms=SXA,
rows=2000;
