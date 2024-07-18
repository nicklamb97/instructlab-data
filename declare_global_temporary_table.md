# Declare Global Temporary Table Statement

The `declare global temporary table` statement creates a temporary table, also known as a session-scope table.

## Syntax

```sql
declare global temporary table session.table_name
          (column_name format {, column_name format})
          [withclause];
```

To create a temporary table by selecting data from another table:

```sql
declare global temporary table session.table_name
          (column_name {, column_name})
          as subselect
          [withclause];
```

### Valid parameters for the `with` clause are:

- `location = (locationname {, locationname})`
- `[no]duplicates`
- `allocation = initial_pages_to_allocate`
- `extend = number_of_pages_to_extend`

For temporary tables created using a subselect, additional parameters in the `with` clause can be specified:

- `structure = hash | heap | isam | btree`
- `key = (columnlist)`
- `fillfactor = n`
- `minpages = n`
- `maxpages = n`
- `leaffill = n`
- `nonleaffill = n`
- `compression[ = ([[no]key][,[no]data])] | nocompression`
- `page_size = n`

Specify multiple `with` clause parameters as a comma-separated list.

## Description

The `declare global temporary table` statement creates a temporary table for manipulating intermediate results, minimizing processing overhead associated with table creation.

Temporary tables have the following characteristics:

- No logging is performed.
- No page locking occurs.
- Disk space is minimized, and tables are often created in memory.
- No system catalog entries are made.
- Tables are visible only to the session that created them and are deleted when the session ends.

## Restrictions on Temporary Tables

Temporary tables have the following restrictions:

- Cannot be used within database procedures.
- Cannot be used in view definitions.
- Cannot have integrities, constraints, or user-defined defaults.
- Cannot use the following SQL statements:
  - create index
  - create permit
  - create synonym
  - create view
  - grant
  - revoke
  - save
  - set journaling
  - set lockmode
  - create security_alarm
  - help
  - create integrity
  - create rule

## Examples

### Example 1

Create a temporary table `emps` with columns `name` and `empno`:

```sql
declare global temporary table
          session.emps
          (name char(20), empno char(5))
          with location = (personnel),
          [no]duplicates,
          allocation = 100,
          extend = 100;
```

### Example 2

Use a subselect to create a temporary table `emps_to_promote` containing names and employee numbers of highly-rated employees:

```sql
declare global temporary table
          session.emps_to_promote
          as select name, empno from employees
          where rating >= 9;
```

*Note:* To delete a temporary table, use `drop table session.table_name`.
