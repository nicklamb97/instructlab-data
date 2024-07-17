Revoke Statement
The revoke statement revokes privileges. It removes database privileges or role access granted to the specified users, groups, roles, or PUBLIC. (To confer privileges, use the grant statement (see Grant (privilege) Statement).) You cannot revoke privileges granted by other users.
This statement has the following syntax:
revoke
          all [privileges] | privilege {, privilege} | object_name {, object_name}
          [on object_type]
          from public | [auth_type] auth_id {, auth_id};
Parameters--Revoke Statement
This statement has the following parameters:
privilege
Specifies the privileges to revoke. To revoke all privileges, use all. The privileges must agree with the object_type as follows:
Object Type
Valid Privileges
database (or current installation)
[no]access
[no]connect_time_limit
[no]create_procedure
[no]create_sequence
[no]create_table
[no]db_admin
[no]idle_time_limit
[no]lockmode
[no]query_cost_limit
[no]query_cpu_limit
[no]query_io_limit
[no]query_page_limit
[no]query_row_limit
[no]select_syscat
[no]session_priority
[no[table_statistics
[no]timeout_abort
[no]update_syscat
object_type
Specifies the type of object on which the privileges were granted. Valid object_types are:
database
current installation
object_name
Specifies the name of the table, database procedure, database event, or role on which the privileges were granted.
auth_type
Specifies the type of authorization identifier to which privileges were granted. Auth_type must be user, group, or role. The default is user. More than one auth_type cannot be specified.
auth_id
Specifies the users, groups, or roles from which privileges are being revoked. The auth_ids must agree with the type specified by the auth_type.
For example, if you specify group as auth_type, the auth_id list must be a list of group identifiers. If you specify PUBLIC for the auth_id, you must omit auth_type. You can revoke from users and PUBLIC in the same revoke statement.
Revoking a database privilege makes that privilege on the specified database undefined for the specified grantee (auth_id). If an attempt is made to revoke a privilege that was not granted to a specified auth_id, no changes are made to the privileges of that auth_id.
Privileges granted on specific databases are not affected by revoke...on current installation, and privileges granted on current installation are not affected by revoke...on database. Revoking privileges from PUBLIC does not affect privileges granted to a specific user.
If a privilege was granted using its “no” form (for example, nocreate_table or noquery_io_limit), the same form must be used when revoking the privilege. For example, the following grant prevents a user from creating tables:
grant nocreate_table on database employee
to user karenk;
To remove this restriction:
revoke nocreate_table on database employee
from user karenk;
For more information about privileges, see Grant (privilege) Statement. For a description of group and role identifiers, and details about privilege dependencies, see the Ingres Database Administrator Guide.
Note:  In some cases granting a database privilege imposes a restriction, and revoking the privilege removes the restriction. For example, grant nocreate_table prevents the user from creating tables.
Revoke Permissions
The revoke statement can be executed by a user who is either the owner of the target object or has been granted permission (using with grant option) to use the statement on the specific target object by another user. To revoke database privileges, you must be working in a session that is connected to the iidbdb. If the indicated roles have security audit attributes, the session must also have MAINTAIN_AUDIT privilege.
Revoke Locking
The revoke statement locks pages in the iidbpriv catalog (if revoking database privileges) or iiprotect catalog, plus pages in the system catalogs that correspond to the object type (table, view, database event, or database procedure).
Revoke Related Statements
Create Group Statement
Create Role Statement
Grant (privilege) Statement
Examples--Revoke Statement
The following is a revoke statement example:
Revoke the query_row_limit privilege defined for the role identifier, review_emp, on the employee database.
revoke query_row_limit on database employee
        from role review_emp;
