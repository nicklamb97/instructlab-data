# Create Role Statement

The `create role` statement defines one or more role identifiers and their associated password. Role identifiers are used to associate privileges with applications. After the role identifiers are created and privileges have been granted to them, use them with the `connect` statement to associate those privileges with the session. For a discussion of role identifiers, see the Ingres Database Administrator Guide.

## Syntax

```
create role role_id {, role_id}
          [with with_option {, with_option}];
```

## Parameters

### role_id
Specifies the role identifier to be created. Must be a valid object name that is unique among all role, group, and user identifier names in the installation. If an invalid role identifier is specified, the DBMS Server returns an error but processes all valid role identifiers. Role identifiers are stored in the iirole catalog of the iidbdb.

### with_option
Specifies one of the following password options:
- `nopassword`
- `password = 'role_password'`
- `external_password`

### nopassword
Specifies that no password is associated with `role_id`.

### password = 'role_password'
Allows setting a password for `role_id`. If `role_password` contains uppercase or special characters, enclose it in single quotes. Any blanks in the password are removed when the password is stored. Limits: `role_password` can be no longer than 24 characters.

### external_password
Allows a user's password to be passed to an external authentication server for authentication.

## Create Role Permissions

You must have `maintain_users` privilege and be connected to the iidbdb database. You must have `maintain_audit` privilege to change security audit attributes.

## Create Role Locking

The `create role` statement locks pages in the iirole catalog of the iidbdb. This can cause sessions attempting to connect to the server to suspend until the statement is completed.

## Create Role Related Statements

- Alter Role Statement
- Drop Role Statement

## Examples

### Example 1
Create a role identifier and password for the inventory application of a bookstore.

```sql
create role bks_onhand with password = 'hgwells';
```

### Example 2
Create a role identifier with no password for the daily sales application of the bookstore.

```sql
create role dly_sales with nopassword;
```
