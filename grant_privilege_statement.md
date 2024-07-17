Grant (privilege) Statement
The grant (privilege) statement grants privileges on the database as a whole or on individual tables, views, sequences or procedures. It controls access to database objects, roles, and DBMS resources.
This statement has the following syntax:
grant all [privileges] | privilege {, privilege}
              [on [object_type] [schema.]object_name {, [schema.]object_name}]
              to public | [auth_type]auth_id{, auth_id};
Details about using the grant statement with role objects is described in grant (role) in the Ingres SQL Reference Guide.
To remove privileges, use the revoke statement (see Revoke Statement). To determine the privileges in effect for a session, use the DBMSINFO function. In some cases granting a privilege imposes a restriction, and revoking the privilege removes the restriction. For example, grant nocreate_table prevents the user from creating tables.
Note:  The grant statement is the ISO/ANSI-compliant method for controlling access to database objects and resources.
To display granted database privileges, select data from the iidbprivileges system catalog. For details about system catalogs, see the Ingres Database Administrator Guide.
Parameters--Grant (privilege) Statement
This statement has the following parameters:
privilege
Specifies the privilege, which must be valid for the object_type.
Valid privileges are described under the section Object Privileges.
object_type
Specifies the type of object on which you are granting privileges. Object_type must be one of the following:
table
(Default) Controls access to individual tables or views.
database
Controls access to database resources.
procedure
Controls who can execute individual database procedures.
dbevent
Controls who can register for and raise specific database events.
current installation
Grants the specified privilege on the current installation.
The object_type must agree with the privilege being granted (for example, EXECUTE privilege cannot be granted on a table).
Privileges cannot be defined for more than one type of object in a single grant statement. If object_type is current installation, object_name must be omitted.
object_name
Specifies the name of the table, view, procedure, database event, sequence or database for which the privilege is being defined. The object must correspond to the object_type. For example, if object_type is table, object_name must be the name of an existing table or view.
auth_type
Specifies the type of authorization to which you are granting privileges. A grant statement cannot contain more than one auth_type. Valid auth_types are:
user
group
role
The auth_ids specified in the statement must agree with the specified auth_type. For example, if you specify auth_type as group, all auth_ids listed in the statement must be group identifiers.
Default: user
public
Grants a privilege to all users. The auth_type parameter can be omitted.
auth_id
Specifies the name of the users, groups, or roles to which you are granting privileges, or PUBLIC. Both PUBLIC and a list of auth_ids can be specified in the same grant statement. If the privilege is subsequently revoked from PUBLIC, the individual privileges still exist.
Object Privileges
The privilege granted depends on the type of object it affects:
•table
•database
•procedure
•dbevent
•sequence
Table Privileges
Table privileges control access to tables and views. By default, only the owner of the table has privileges for the table. To enable others to access the table, the owner must grant privileges to specific authorization IDs or to public. Table privileges must be granted explicitly.
Valid table privileges are:
select
Allows grantee to select rows from the table.
insert
Allows grantee to add rows to the table.
update
Allows grantee to change existing rows. To limit the columns that the grantee can change, specify a list of columns to allow or a list of columns to exclude.
To grant the privilege for specific columns, use the following syntax after the update keyword in the grant statement:
(column_name {, column_name})
To grant the privilege for all columns except those specified, use the following syntax after the update keyword in the grant statement:
excluding (column_name {, column_name})
If the column list is omitted, update privilege is granted to all columns of the table or, for views, all updatable columns.
delete
Allows grantee to delete rows from the table.
all [privileges]
Grants the subset of select, insert, update, delete, and references privileges for which the grantor has GRANT OPTION. For details, see Grant All Privileges Option.
When privileges are granted against a table, the date and timestamp of the specified table is updated, and the DBMS Server recreates query plans for repeat queries and database procedures that see the specified table.
Table Privileges for Views
The privileges required to enable the owner of a view to grant privileges on the view are as follows:
select
View owner must own all tables and views used in the view definition, or view owner or public must have grant option for select for the tables and views used in the view definition.
insert
View owner must own all tables and views used in the view definition, or view owner or public must have grant option for insert for the tables and views used in the view definition.
update
View owner must own all tables and updatable columns in views used in the view definition, or view owner or public must have grant option for update for the tables and updatable columns in views used in the view definition.
delete
View owner must own all tables and views used in the view definition, or view owner or public must have grant option for delete for the tables and views used in the view definition.
To grant privileges for views the grantor does not own, the grantor must have been granted the specified privilege with grant option.
Database Privileges
Database privileges control the consumption of computing resources.
To override the default for a database privilege, grant a specific value to PUBLIC. For example, by default, everyone (PUBLIC) has the privilege to create database procedures. To override the default, grant NOCREATE_PROCEDURE to PUBLIC, and grant the CREATE_PROCEDURE privilege to any user, group, or role that you want to have this privilege. (Users, groups, and roles are referred to collectively as authorization IDs.)
Database privileges do not apply to DBAs in their own databases, nor to security administrators.
The database privileges in effect for a session are determined by the values that were granted to the authorization IDs in effect for the session, according to the following hierarchy:
1.Role
2.User
3.Group
4.Public
For example, if different values for QUERY_ROW_LIMIT are granted to PUBLIC, and to the user, group, and role that are in effect for a session, the value for the role of the session prevails.
Valid database privileges are as follows:
[no]access
Allows the specified authorization IDs to connect to the specified database.
Noaccess prevents the specified authorization IDs from connecting.
Note:  The access bitmask of the iidatabase_info catalog is set only if Ingres utilities are used to change the access rights to the database. Using a grant [no]access on database dbname to public statement does not change the access from global to private or vice versa in iidatabase_info.
[no]create_procedure
Allows the specified authorization IDs to create database procedures in the specified database.
Nocreate_procedure prevents the specified users, groups, or roles from creating database procedures.
Default: All authorization IDs can create database procedures.
[no]create_sequence
Allows the specified authorization IDs to create, alter and drop sequences in the specified database.
Nocreate_sequence prevents the specified authorization IDs from creating sequences. By default, all authorization IDs can create, alter and drop sequences.
[no]create_table
Allows the specified authorization IDs to create tables in the specified database.
Nocreate_table prevents the specified authorization IDs from creating tables.
Default: All authorization IDs can create tables.
[no]db_admin
Confers unlimited database privileges for the specified database and the ability to specify effective user (using the -u flag). A session that has the DB_ADMIN privilege does not have all the rights that a DBA has; some utilities can be run only by a DBA. The DBA of a database and users with the SECURITY privilege have the DB_ADMIN privilege by default. For all other users, the default is nodb_admin.
[no]lockmode
Allows the specified authorization IDs to issue the set lockmode statement (see Lockmode).
Nolockmode prevents the specified users, groups, or roles from issuing the set lockmode statement.
Default: Everyone can issue the set lockmode statement.
[no]update_syscat
Allows the specified authorization IDs to update system catalogs when working in a session connected to the iidbdb.
[no]select_syscat
Allows a session to query system catalogs to determine schema information. When connected to the iidbdb database, this includes the master database catalogs such as iiuser and iidatabase. Select_syscat can be granted to user, group, role or public, and can only be issued when connected to the iidbdb database.
This privilege restricts user queries against the core DBMS catalogs containing schema information, such as iirelation and iiattribute. Standard system catalogs such as iitables can still be queried.
[no]session_priority
Determines whether a session is allowed to change its priority, and if so, its initial and highest priority.
If nosession_priority (the default) is specified, users can not alter their session priority.
If session_priority is specified, users can alter their session priority, up to the limit determined by the privilege.
[no]table_statistics
Allows users to view (by way of SQL and statdump) and create (by way of optimizedb) database table statistics.
If statistics exist in the database catalogs the DBMS Server automatically uses them when processing queries, even if the user does not possess this privilege.
[no]timeout_abort
Allows the specified authorization IDs to issue the set joinop timeoutabort statement (see Joinop Timeoutabort).
Notimeout_abort prevents the specified users, groups, or roles from issuing the set joinop timeoutabort statement.
Default: Everyone can issue the set joinop timeoutabort statement.
Procedure Privileges
The EXECUTE privilege allows the grantee to execute the specified database procedures. To grant the EXECUTE privilege on database procedures, the owner of the procedure must have GRANT OPTION for all the privileges required to execute the procedure. To grant the EXECUTE privilege on database procedures that the grantor does not own, the grantor must have EXECUTE privilege WITH GRANT OPTION for the database procedure.
Dbevent Privileges
Database event privileges are as follows:
raise
Allows the specified authorization IDs to raise the database event (using the raise dbevent statement (see Raise Dbevent Statement))
register
Allows the specified authorization IDs to register to receive a specified database event (using the register dbevent statement (see Register Dbevent Statement))
Privilege Defaults
Privilege defaults are as follows:
Privilege
Default
SELECT
INSERT
DELETE
UPDATE
Only the owner can perform select, insert, delete, or update operations on objects it owns.
REFERENCES
Only the table owner can create referential constraints that see its tables.
EXECUTE
Only the owner of a database procedure can execute the procedure.
RAISE
Only the owner of a database event can raise the event.
REGISTER
Only the owner of a database event can register to receive the event.
NEXT
Only the owner of a database sequence can execute the next value and current value operators on the sequence.
Database privilege defaults are as follows:
Privilege
Description
QUERY_IO_LIMIT
Any user can perform unlimited I/O (noquery_io_limit)
QUERY_ROW_LIMIT
Any user can obtain unlimited rows (noquery_row_limit).
CREATE_TABLE
Any user can create tables (create_table).
CREATE_PROCEDURE
Any user can create database procedures (create_procedure).
CREATE_SEQUENCE
Any user can create database sequences (create_sequence).
LOCKMODE
Any user can issue the set lockmode statement (lockmode).
DB_ADMIN
For a specified database, the DBA of the database and users that have the security privilege have the db_admin privilege. All other users of the database have nodb_admin privilege.
Grant All Privileges Option
The following sections describe the results of the grant all privileges option.
Installation and Database Privileges
If GRANT ALL PRIVILEGES ON DATABASE or GRANT ALL PRIVILEGES ON CURRENT INSTALLATION is specified, the grantees receive the following database privileges:
•NOQUERY_IO_LIMIT
•NOQUERY_ROW_LIMIT
•CREATE_TABLE
•CREATE_PROCEDURE
•LOCKMODE
•RAISE DBEVENT
•REGISTER DBEVENT
Privileges granted on a specific database override privileges granted on current installation.
Other Privileges
The requirements for granting all privileges on tables, views, database procedures, and database events depend on the type of object and the owner. To grant a privilege on an object owned by another user, the grantor or public must have been granted the privilege with grant option. Only the privileges for which the grantor or public has GRANT OPTION are granted.
The following example illustrates the results of the grant all privileges option. The accounting_mgr user creates the following employee table:
create table employee (name char(25), department char(5),
salary money)...
and, using the following grant statement, grants the accounting_supervisor user the ability to select all columns but only allows accounting_supervisor to update the department column (to prevent unauthorized changes of the salary column):
grant select, update (department) on table employees to accounting_supervisor;
If the accounting_supervisor user issues the following grant statement:
grant all privileges on table employees to accounting_clerk;
the accounting_clerk user receives select and update (department) privileges.
Granting All Privileges on Views
The results of granting all privileges on a view you do not own are determined as follows:
Privilege
Results
SELECT
Granted if the grantor can grant SELECT privilege on all tables and views in the view definition.
UPDATE
Granted for all columns for which the grantor can grant UPDATE privilege. If the grantor was granted UPDATE...WITH GRANT OPTION on a subset of the columns of a table, UPDATE is granted only for those columns.
INSERT
Granted if the grantor can grant INSERT privilege on all tables and views in the view definition.
DELETE
Granted if the grantor can grant DELETE privilege on all tables and views in the view definition.
REFERENCES
The REFERENCES privilege is not valid for views.
Grant (privilege) Permissions
Database privileges are not enforced if the user has the SECURITY privilege or is the DBA of the current database.
The grant statement can be executed by a user who is either the owner of the target object, or has been granted permission (using with grant option) to use the statement on the specific target object by another user.
Grant (privilege) Locking
Granting privileges on a table takes an exclusive lock on that table. Granting privileges on the database as a whole locks pages in the iidbpriv catalog of the iidbdb.
Grant (privilege) Related Statements
Create Dbevent Statement
Create Group Statement
Create Table Statement
Create View Statement
Revoke Statement
Examples--Grant (privilege) Statement
The following are grant (privilege) statement examples:
1.Grant select and update privileges on the salary table to the group, acct_clerk.
grant select, update on table salary
    to group acct_clerk;
2.Grant update privileges on the columns, empname and empaddress, in the employee table to the users, joank and gerryr.
grant update(empname, empaddress)
    on table employee
    to joank, gerryr;
3.Grant permission to the public to execute the monthly_report procedure.
grant execute on procedure monthly_report
    to public;
4.Grant unlimited rows to the role identifier, update_emp, which allows unlimited rows to be returned to any application that is associated with the role identifier, update_emp.
grant noquery_row_limit on database new_acct
    to role update_emp;
5.Enable the inventory_monitor role to register for and raise the stock_low database event.
grant register, raise on dbevent stock_low
    to role inventory_monitor;
6.Enable any employee in accounting to change columns containing salary information.
grant update on employee.salary, employee.socsec
    to group accounting;
7.Enable any user to create a table constraint that references the employee roster.
grant references on emp_roster to public;
