# Drop Integrity Statement

The drop integrity statement removes the specified integrity constraints from a table.

## Syntax

```
drop integrity on table_name ALL | integer {, integer};
```

When integrities are dropped from a table, the DBMS Server updates the date and timestamp of that table.

After integrities are dropped from a table, the DBMS Server recreates query plans for repeat queries and database procedures when an attempt is made to execute the repeat query or database procedure.

**Note:** The drop integrity statement does not remove constraints defined using the create table statement (see Create Table Statement).

## Parameters

This statement has the following parameters:

- **table_name**: Specifies the name of the table for which integrity constraints are to be dropped.
- **all**: Removes all the constraints currently defined for the specified table.
- **integer (,integer)**: Removes individual constraints. To obtain the integer equivalents for integrity constraints, execute the help integrity statement.

## Permissions

You must own the table.

## Related Statements

- Create Integrity Statement

## Examples

The following are drop integrity statement examples:

1. Drop integrity constraints 1, 4, and 5 on the job table.

    ```
    drop integrity on job 1, 4, 5;
    ```

2. In an application, drop all the constraints against the exhibitions table.

    ```
    drop integrity on exhibitions all;
    ```
