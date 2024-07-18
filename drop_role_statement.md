# Drop Role Statement

The drop role statement removes the specified role identifiers from the installation. Any session using a role identifier when the identifier is dropped continues to run with the privileges defined for that identifier. For more information about role identifiers, see the Ingres Database Administrator Guide.

## Syntax

```
drop role role_id {, role_id};
```

## Parameters

This statement has the following parameters:

- **role_id**: Specifies an existing role identifier. If the list of role_ids contains any that do not exist, the DBMS Server returns an error for each non-existent role_id, but does not abort the statement. Others in the list that are valid, existing role identifiers, are removed.

## Permissions

You must have MAINTAIN_USERS privilege and be connected to the iidbdb database.

## Locking

The drop role statement locks pages in the iirole catalog in the iidbdb.

## Related Statements

- Create Role Statement
- Alter Role Statement

## Examples

The following example drops the sales_report role identifier:

```
drop role sales_report;
```
