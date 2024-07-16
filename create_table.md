Create Table Statement
The create table statement creates a base table.
Note:  This statement has additional considerations when used in a distributed environment. For more information, see the Ingres Star User Guide.
This statement has the following syntax:
create table [schema.] table_name
              (column_specification {, column_specification})
              [with with_clause];
The create table...as select statement (which creates a table and load rows from another table) has the following syntax:
create table table_name
              (column_name {, column_name}) as
                            subselect
                            {union [all]
                            subselect}
              [with with_clause];
Parameters--Create Table Statement
This statement has the following parameters:
table_name
Defines the name of the new table, which must be a valid object name.
column_specification
Defines the characteristics of the column, as described in Column Specification--Define Column Characteristics.
with_clause
Consists of a comma-separated list of one or more of the following options, described in detail in With_Clause for Create Table:
•location = (location_name {, location_name})
•[no]journaling
•[no]duplicates
•page_size = n
•security_audit = (audit_opt {, audit_opt})
•security_audit_key = (column)
•[no]autostruct
•encryption = AES128|AES192|AES256, [aeskey=hex-aes-key,] passphrase = 'encryption-passphrase'
Additional options on the With_Clause for Create Table...As Select are as follows:
•structure = HASH | HEAP | ISAM | BTREE
•key = (column_name {, column_name})
•fillfactor = n
•minpages = n
•maxpages = n
•leaffill = n
•nonleaffill = n
•compression[= ([[NO]KEY] [,[NO]DATA])] | NOCOMPRESSION
•allocation = n
•extend = n
•priority = n
subselect
Specifies a select clause, described in detail in Select (interactive) in the Ingres SQL Reference Guide. Also see Using Create Table...As Select.
Note:  Subselect cannot be used when creating a table in one or more raw locations—that is, create table raw_table as select... with location = (raw_loc).
Create Table Description
The create table statement creates a base table. A base table contains rows independently of other tables (unlike a view, which has no independent existence of its own). Rows in a base table are added, changed, and deleted by the user (unlike an index, which is automatically maintained by the DBMS Server).
The default page size is the smallest of either 8K (8192 bytes)—unless changed by the system administrator—or the page size configured in the installation that holds the record. For example, if 2K (2048 bytes), 4K (4096 bytes), and 8K (8192 bytes) page sizes are configured and the row size for a table to be created is 2500 bytes, the table is created with an 8K page size. Or, if 2K, 8K and 16K bytes page sizes are configured and the row size for the table to be created is 12,000 bytes, the table is created with a 16K page size.
Note:  If the row is larger than any page size configured, or if a page size too small is specified with the page_size clause, Ingres creates the table, but the larger rows will span multiple pages.
The default storage structure for tables is either B-tree or heap, depending on the setting of the table_auto_structure configuration parameter or with [no]autostruct in combination with the presence of constraint definitions in the create table statement.
To create a table that is populated with data from another table, specify create table...as select. The resulting table contains the results of the select statement.
By default, tables are created without an expiration date. To specify an expiration date for a table, use the save statement. To delete expired tables, use the verifydb utility. For details, see the Ingres System Administrator Guide.
A maximum of 1024 columns can be specified for a base table.
The following table shows the maximum row length when rows do not span pages.
Page Size
Max Row Length
2048 (2 KB)
2008 bytes
4096 (4 KB)
3988 bytes
8192 (8 KB)
8084 bytes
16384 (16 KB)
16276 bytes
32768 (32 KB)
32660 bytes
65536 (64 KB)
65428 bytes
You can create a table with row size greater than the maximum documented above, up to 256 KB. If the with page_size clause is not specified, the table is created with the default page size.
Note:  Ingres is more efficient when row size is less than or equal to the maximum row size for the corresponding page size.
Long varchar and long byte columns can contain a maximum of 2 GB characters and bytes, respectively. The length of long varchar or long byte columns cannot be specified.
The following data types require space in addition to their declared size:
•A varchar or text column consumes two bytes (in addition to its declared length) to store the length of the string.
•Nullable columns require one additional byte to store the null indicator.
•In tables created with compression, c columns require one byte in addition to the declared length, and char columns require two additional bytes.
Note:  If II_DECIMAL is set to comma, you must follow any comma required in SQL syntax (such as a list of table columns or SQL functions with several parameters) by a space. For example:
select col1, ifnull(col2, 0), left(col4, 22) from t1:
Column Specification--Define Column Characteristics
The column specification in a create table statement defines the characteristics of a column in the table.
The column_specification has the following format:
column_name datatype
[with null | not null]
[with default default_spec | with default | not default]
column_name
Assigns a valid name to the column.
datatype
Assigns a valid data type to the column. If create table...as select is specified, the new table takes its column names and formats from the results of the select clause of the subselect specified in the as clause (unless different column names are specified).
Note:  For char and varchar columns, the column specification is in number of bytes (not number of characters).
default clause
Specifies whether the column is mandatory, as described in Default Clause.
null clause
Specifies whether the column accept nulls, as described in Null Clause.
Default Clause
The with|not default clause in the column specification specifies whether a column requires an entry.
This clause has the following format:
with default default_spec | with default | not default
not default
Indicates the column is mandatory (requires an entry).
with default
Indicates that if no value is provided, the DBMS Server inserts 0 for numeric and money columns, an empty string for character columns, an empty string for Ingres date columns, and the current date for ANSI date columns.
with default default_spec
Indicates that if no value is provided (because none is required), the DBMS Server inserts the default value. The default value must be compatible with the data type of the column.
For character columns, valid default values include the following constants:
–user
–current_user
–system_user
For boolean columns, valid default values include FALSE or TRUE.
The following is an example of using the default clause:
create table dept(dname    char(10),
    location      char(10)   not null with  default 'NY',
    creation      date       not null with  default '01/01/03',
    budget        money      not null with  default 10000);
Restrictions on the Default Value for a Column
The following considerations and restrictions apply when specifying a default value for a column:
•The data type and length of the default value must not conflict with the data type and length of the column.
•The maximum length for a default value is 1500 characters.
•For fixed-length string columns, if the column is wider than the default value, the default value is padded with blanks to the declared width of the column.
•For numeric columns that accept fractional values (floating point and decimal), the decimal point character specified for the default value must match the decimal point character in effect when the value is inserted. To specify the decimal point character, set II_DECIMAL.
•For money columns, the default value can be exact numeric (integer or decimal), approximate numeric (floating point), or a string specifying a valid money value. The decimal point and currency sign characters specified in the default value must match those in effect when the value is inserted.
•For date columns, the default value must be a string representing a valid date. If the time zone is omitted, the time zone defaults to the time zone of the user inserting the row.
•For user-defined data types (UDTs), the default value must be specified using a literal that makes sense to the UDT. A default value cannot be specified for a logical key column.
Null Clause
The with|not null clause in the column specification specifies whether a column accepts null values.
This clause has the following format:
with null | not null [with default [default_spec]]
with null
Indicates that the column accepts nulls. If no value is supplied by the user, null is inserted. With null is the default for all data types except a SYSTEM_MAINTAINED logical key.
not null
Indicates that the column does not accept nulls.
With|Not Null and With|Not Default Combinations
The with|not null clause works in combination with the with|not default clause, as follows:
with null
The column accepts nulls. If no value is provided, the DBMS Server inserts a null.
with null with default
The column accepts null values. If no value is provided, the DBMS Server inserts a 0 or blank string, depending on the data type.
with null not default
The column accepts null values. The user must provide a value (mandatory column).
not null with default
The column does not accept nulls. If no value is provided, the DBMS Server inserts 0 for numeric and money columns, or an empty string for character and date columns.
not null not default (or not null)
The column is mandatory and does not accept nulls, which is typical for primary key columns.
System_Maintained Logical Keys
SYSTEM_MAINTAINED logical key columns are assigned values by the DBMS Server, and cannot be assigned values by applications or end users. The following restrictions apply to logical keys specified as with SYSTEM_MAINTAINED:
•The only valid default clause is with default. If the default clause is omitted, with default is assumed.
•The only valid null clause is not null. If a column constraint or null clause is not specified, not null is assumed.
•No table constraint can include a SYSTEM_MAINTAINED logical key column. For details about table constraints, see Column-Level Constraints and Table-Level Constraints in the Ingres SQL Reference Guide.
System_Maintained Clause
Null Clause
Valid?
with SYSTEM_MAINTAINED
not null
yes
 
with null
no
 
not nullwith default
yes
 
not null not default
no
 
(none specified)
yes
not SYSTEM_MAINTAINED
not null
yes
 
with null
yes
 
not null with default
yes
 
not null not default
yes
Using Create Table...As Select
The create table...as select syntax creates a table from another table or tables. The new table is populated with the set of rows resulting from execution of the specified select statement.
Note:  The create table...as select syntax is an Ingres extension and not part of the ANSI/ISO Entry SQL-92 standard.
By default, the storage structure of the table is heap with compression. To override the default, issue the set result_structure statement prior to issuing the create table...as select statement or specify the with structure option.
By default, the columns of the new table have the same names as the corresponding columns of the base table from which you are selecting data. Different names can be specified for the new columns.
The data types of the new columns are the same as the data types of the source columns. The nullability of the new columns is determined as follows:
•If a source table column is nullable, the column in the new table is nullable.
•If a source table column is not nullable, the column in the new table is defined as not null.
If the source column has a default value defined, the column in the new table retains the default definition. However, if the default value in the source column is defined using an expression, the default value for the result column is unknown and its nullability depends on the source columns used in the expression. If all the source columns in the expression are not nullable, the result column is not nullable. If any of the source columns are nullable, the result column is nullable. Also see Default Type Conversion.
A SYSTEM_MAINTAINED logical key column cannot be created using the create table...as select syntax. When creating a table using create table...as select, any logical key columns in the source table that are reproduced in the new table are assigned the format of NOT SYSTEM_MAINTAINED.
With_Clause for Create Table
Create table accepts the following options on the with clause, specified as a comma-separated list:
location = (location_name {, location_name})
Specifies the locations where the new table is created. To create locations, use the create location statement (see the Ingres SQL Reference Guide). The location_names must exist and the database must have been extended to the corresponding areas. If the location option is omitted, the table is created in the default database location. If multiple location_names are specified, the table is physically partitioned across the areas. For details about defining location names and extending databases, see the Ingres Database Administrator Guide.
[no]journaling
Explicitly enables or disables journaling on the table. For details about journaling, see the Database Administrator Guide.
To set the session default for journaling, use the set [no]journaling statement (see [No]Journaling). The session default specifies the setting for tables created during the current session. To override the session default, specify the with [no]journaliing clause in the create table statement.
If journaling is enabled for the database and a table is created with journaling enabled, journaling begins immediately. If journaling is not enabled for the database and a table is created with journaling enabled, journaling begins when journaling is enabled for the entire database.
Note:  To enable or disable journaling for the database and for system catalogs, use the ckpdb command. For information about ckpdb, see the Ingres Command Reference Guide.
[no]duplicates
Allows or disallows duplicate rows in the table. This option does not affect a table created as heap. Heap tables always accept duplicate rows regardless of the setting of this option.
If a heap table is created with noduplicates and is subsequently modified to a different table structure, the noduplicates option will be enforced with the result that rows which are totally identical will have the duplicates dropped without warning.
The duplicates setting can be overridden by specifying a unique key for a table in the modify statement (see Modify Statement).
Default: duplicates
page_size = n
Specifies a page size, in number of bytes. Valid values are described in Page_size Option.
Default: 2048. The tid size is 4.
The buffer cache for the installation must also be configured with the page size specified in create table or an error occurs.
security_audit = (audit_opt {, audit_opt})
Specifies row or table level auditing, as described in Security_audit Option.
security_audit_key = (column)
Writes an attribute to the audit log to uniquely identify the row in the security audit log. For example, an employee number can be used as the security audit key.
[no]autostruct
Specifies whether the storage structure of the table should automatically default to B-tree (with autostruct) or to heap (with noautostruct), in combination with constraint definitions in create table.
encryption=encryption type, [aeskey=hex-aes-key,] passphrase='encryption-passphrase'
Specifies encryption options to secure data in a column defined with the ENCRYPT option:
encryption type
Specifies the type of Advanced Encryption Standard (AES) encryption for the column:
AES128
128-bit encryption
AES192
192-bit encryption
AES256
256-bit encryption
aeskey=
Specifies the internal key used to encrypt the user data. The key is protected by a key derived from the passphrase. The aeskey must be the exact hex value of the key, which depends on whether AES128, AES192, or AES156 is specified (thus 16, 24, or 32 bytes) and is in the usual Ingres hex constant format of x'03010401050902060503050901040104' or 0x03010401050902060503050901040104.
If no aeskey is specified, a random key is generated.
passphrase='encryption passphrase'
Specifies the encryption passphrase used to encrypt and decrypt the column data. The passphrase must be at least eight characters, can contain spaces, and must be specified within single quotation marks (' '). The string is converted to the appropriate AES key size (16, 24, or 32 bytes).
Page_size Option
The page_size option on the with clause in the create table statement creates a table with a specific page size. This option has the following format:
page_size = n
where n is the number of bytes.
Valid values are shown in the Number of Bytes column in the following table:
Page Size
Number of Bytes
Page Header
2K
2,048
40
4K
4,096
76
8K
8,192
76
16K
16,384
76
32K
32,768
76
64K
65,536
76
Security_audit Option
The security_audit option on the with clause specifies the auditing level.
This option has the following format:
security_audit = (table_audit_opt {, table_audit_opt})
table_audit_opt
Specifies the level of security, as follows:
table
(Default) Implements table-level security auditing on general operations (for example create, drop, modify, insert, or delete) performed on the table.
[no]row
Implements row-level security auditing on operations performed on individual rows, such as insert, delete, update, or select. If norow is specified, the row-level security auditing is not implemented.
For example, an SQL delete statement that deleted 500 rows from a table with both table and row auditing generates the following audit events:
•One table-delete audit event, indicating the user issued a delete against the table.
•500 row-delete audit events, indicating which rows were deleted.
Note:  Either table and row or table and norow auditing can be specified. If norow is specified, row-level auditing is not performed. If either clause is omitted, the default installation row auditing is used. The default can be either row or norow depending on how your installation is configured.
With Security_audit_key Clause
The with security_audit_key clause allows the user to specify an optional attribute to be written to the audit log to assist row or table auditing. For example, an employee number can be used as the security audit key:
create table employee (name char(60), emp_no integer)
with security_audit = (table, row),
        security_audit_key = (emp_no);
If no user-specified attribute is given and the table has row-level auditing, a new hidden attribute, _ii_sec_tabkey of type TABLE_KEY SYSTEM_MAINTAINED is created for the table to be used as the row audit key. Although any user attribute can be used for the security audit key (security_audit_key clause), we recommend that a short, distinctive value be used (such as a social security ID), allowing the user to uniquely identify the row when reviewing the security audit log. If an attribute longer than 256 bytes is specified for the security audit key, only the first 256 bytes are written to the security audit log.
With_Clause for Create Table...As Select
Create table...as select accepts the following options on the with clause, specified as a comma-separated list:
allocation = n
Specifies the number of pages initially allocated for the table.
Limits: Integer between 4 and 8,388,607.
Default: 4
extend = n
Specifies the number of pages by which the table is extended when more space is required.
Limits: Integer between 1 and 8,388,607
Default: 16
structure = structure
Specifies the storage structure of the new table. Valid values include: BTREE, ISAM, HEAP, or HASH.
key = (column_name {, column_name})
Specifies the columns on which your table is keyed. All columns in this list must also be specified in the subselect. Be advised that this option affects the way data is physically clustered on disk.
fillfactor = n
Specifies the percentage of each primary data page that must be filled with rows (under ideal conditions). Fillfactor is not valid if a heap table is being created.
Note:  Large fillfactors in combination with a non-uniform distribution of key values can cause a table to contain overflow pages, increasing the time required to access the table.
Limits: 1 to 100
minpages = n
Specifies the minimum number of primary pages a hash table must have when created.
Limits: Minimum value is 1. Cannot exceed the value of maxpages, if specified.
maxpages = n
Specifies the maximum number of primary pages a hash table can have when created.
Limits: Minimum value is 1.
compression [= ([[no]key] [,[no]data])] | nocompression
Specifies whether the key or data is to be compressed. If compression is specified, the structure clause must be specified.
leaffill = n
(B-tree tables only) Specifies the maximum percentage full for leaf index pages. Leaf pages are the index pages directly above the data pages.
Default: 70
nonleaffill = n
(B-tree tables only) Specifies the maximum percentage full for nonleaf index pages.
Default: 80
priority = n
Specifies cache priority. If an explicit priority is not set for an index belonging to a base table to which an explicit priority has been assigned, the index inherits the priority of the base table.
Limits: Integer between 0 and 8, with 0 being the lowest priority and 8 being the highest.
Default: 0. (Causes the table to revert to a normal cache management algorithm.)
Create Table Permissions
This statement is available to all users.
Using the grant statement, the DBA can control whether specific users, groups, or roles can create tables.
Create Table Locking
The DBMS Server takes an exclusive table lock when creating a table, which prevents other sessions—even those using readlock=nolock—from accessing the table until the transaction containing the create table statement is committed.
Create Table Related Statements
Create Integrity Statement
Drop Statement
Grant (privilege) Statement
Modify Statement
[No]Journaling
Examples--Create Table Statement
The following are create table statement examples:
1.Create the employee table with columns eno, ename, age, job, salary, and dept, with journaling enabled.
create table employee
   (eno smallint,
    ename varchar(20) not null with default,
    age integer1,
    job smallint,
    salary float4,
    dept smallint)
   with journaling;
2.Create a table listing employee numbers for employees who make more than the average salary.
create table highincome AS
    select eno
    from employee
    where salary >
    (select avg (salary)
     from employee);
3.Create a table that spans two locations. Specify number of pages to be allocated for the table.
create table emp as
    select eno from employee
    with location = (location1, location2),
    allocation = 1000;
4.Create a table specifying defaults.
create table dept (
    dname char(10),
    location char(10) not null with default 'LA'
    creation_date date not null with default '1/1/99',
    budget money not null with default 100000,
    expenses money not null with default 0);
5.Base table structure is hash unique on dept_id.
create table department (dept_id char(6) not null, dept_name char(20));
modify department to hash unique on dept_id;
