# Alter Role Statement

The alter role statement changes the attributes associated with a role identifier.

This statement has the following syntax:

```sql
alter role role_id {, role_id} [with with_option {, with_option}];
```

## Parameters--Alter Role Statement

This statement has the following parameters:

### *role_id*

Specifies an existing role ID created with the create role statement. If one or more of the specified role identifiers do not exist, the DBMS Server issues a warning, but all valid role identifiers are processed.

For more information about role identifiers, see the Ingres *Database Administrator Guide*.

### *role_password*

Defines the password for the role.

**Caution!** If no password is specified, any session has access to the specified role identifier and its associated permissions.

### *with_option*

Specifies one of the following password options:

- **nopassword**
- **password = '*role_password*'**

## Permissions

You must have maintain_users privilege and be connected to the iidbdb database.

You must have maintain_audit privilege to change security audit attributes.

## Locking

The alter role statement locks pages in the iirole catalog of the iidbdb. This can cause sessions attempting to connect to the server to suspend until the statement is completed.

## Related Statements

- Create Role Statement
- Drop Role Statement

## Examples--Alter Role Statement

The following examples change the attributes associated with a role identifier:

1. Change the password for the role identifier, new_accounts, to eggbasket.

```sql
alter role new_accounts with
    password = 'eggbasket';
```

2. Remove the password associated with the identifier, chk_inventory.

```sql
alter role chk_inventory with nopassword;
```
