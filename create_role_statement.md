Create Role Statement
The create role statement defines one or more role identifiers and their associated password. Role identifiers are used to associate privileges with applications. After the role identifiers are created and privileges have been granted to them, use them with the connect statement to associate those privileges with the session. For a discussion of role identifiers, see the Ingres Database Administrator Guide.
Only users who have been granted access to a role can use a role. The creator of a role is automatically granted access to that role.
This statement has the following syntax:
create role role_id {, role_id}
          [with with_option {, with_option}];
Parameters--Create Role Statement
This statement has the following parameters:
role_id
Specifies the user name to be created. Must be a valid object name that is unique among all role, group, and user identifier names in the installation.
If an invalid role identifier is specified, the DBMS Server returns an error but processes all valid role identifiers.
Role identifiers are stored in the iirole catalog of the iidbdb. For details about system catalogs, see the Ingres Database Administrator Guide.
with_option
Specifies one of the following password options:
nopassword
password = 'role_password'
role_password
Allows a user to change his password. In addition, users with the MAINTAIN_USERS privilege can change or remove any password. If role_password contains uppercase or special characters, enclose it in single quotes. Any blanks in the password are removed when the password is stored.
Limits: Role_password can be no longer than 24 characters
Default: Nopassword if the password clause is omitted.
To remove the password associated with role_id, specify nopassword.
To allow a user's password to be passed to an external authentication server for authentication, specify external_password.
Create Role Permissions
You must have maintain_users privilege and be connected to the iidbdb database.
You must have maintain_audit privilege to change security audit attributes.
Create Role Locking
The create role statement locks pages in the iirole catalog of the iidbdb. This can cause sessions attempting to connect to the server to suspend until the statement is completed.
Create Role Related Statements
Alter Role Statement
Drop Role Statement
Examples--Create Role Statement
The following are create role statement examples:
1.Create a role identifier and password for the inventory application of a bookstore.
create role bks_onhand with password = 'hgwells';
2.Create a role identifier with no password for the daily sales application of the bookstore.
create role dly_sales with nopassword;
