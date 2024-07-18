# Drop Rule Statement

The drop rule statement removes the specified rule from the database. (A rule is dropped automatically if the table on which the rule is defined is dropped.)

## Syntax

```
drop rule [schema.]rulename;
```

## Parameters

This statement has the following parameter:

- **rulename**: Specifies the name of the rule to drop.

## Permissions

You must be the owner of a rule.

## Examples

The following example drops the chk_name rule:

```
drop rule chk_name;
```
