# Create Index Statement

The `create index` statement creates an index on an existing table.

## Syntax

```
create [unique] index [schema.]index_name
              on [schema.]table_name
              (column_name {, column_name})
              [with with_clause];
```

## Parameters

### index_name
Defines the name of the index. This must be a valid object name.

### table_name
Specifies the table on which the index is to be created.

### column_name
Specifies a list of columns from the table to be included in the index. If the key option is used, the columns specified as keys must head this list and must appear in the same order in which they are specified in the key option. If the structure is `rtree`, only one column can be named.

### structure = BTREE | ISAM | HASH | RTREE
Specifies the storage structure of the index. Defaults to `isam` if the parameter is not included. If the structure is `rtree`, `unique` cannot be specified.

**Default:** ISAM

### with with_clause
Specifies a comma-separated list of any of the following items:

- **key = (columnlist)**
  Specifies the columns on which the index is keyed. If this parameter is not included, the index is keyed on the columns in the index definition. If the structure is `rtree`, only one column can be named.

- **fillfactor = n**
  Specifies the percentage of each primary data page that can be filled with rows.
  - **Limits:** 1 to 100 and must be expressed as an integer literal or integer variable.
  - **Default:** Default values differ for each storage structure.

- **minpages = n**
  Defines the minimum number of primary pages a hash or compressed hash index table must have. The value can be expressed as an integer literal or integer variable.
  - **Default:** 16 for a hash table; 1 for a compressed hash table.

- **maxpages = n**
  Defines the maximum number of primary pages that a hash or compressed hash index can have. The value can be expressed as an integer literal or integer variable.
  - **Default:** No default

- **leaffill = n**
  Defines the percentage full each leaf index page is when the index is created. This option can be used when creating an index with a `btree` or compressed `btree` structure.
  - **Limits:** 1 to 100 and must be an integer literal or integer variable.

- **nonleaffill = n**
  Specifies the percentage full each nonleaf index page is when the index is created. This option can be used when creating an index with a `btree` or compressed `btree` structure.
  - **Limits:** 1 to 100, and must be an integer literal or integer variable.

- **location = (location_name {, location_name})**
  Specifies the areas on which the index is created. `location_name` must be a string literal or string variable.
  - **Default:** The default area for the database

- **allocation = n**
  Specifies the number of pages initially allocated for the index.
  - **Limits:** An integer between 4 and 8,388,607
  - **Default:** 4

- **extend = n**
  Specifies the number of pages by which the index is extended when more space is required.
  - **Limits:** An integer between 1 and 8,388,607
  - **Default:** 16

- **compression | nocompression**
  Specifies whether the index key and data are to be compressed. If the structure is `RTREE`, compression cannot be specified.
  - **Default:** NOCOMPRESSION

- **[no]persistence**
  Specifies whether the modify statement recreates the index when its base table is modified.
  - **Default:** nopersistence (indexes are not recreated).

- **unique_scope = STATEMENT | ROW**
  For unique indexes only. Specifies whether rows are checked for uniqueness one-by-one as they are inserted or after the update is complete. If the structure is `rtree`, `unique_scope` cannot be specified.
  - **Default:** unique_scope = row

- **page_size = n**
  Specifies page size.

- **priority = cache_priority**
  Allows tables to be assigned fixed priorities.
  - **Limits:** An integer between 0 and 8

> **Note:** If `II_DECIMAL` is set to comma, be sure that when SQL syntax requires a comma (such as a list of table columns or SQL functions with several parameters), that the comma is followed by a space. For example:
>
> ```sql
> select col1, ifnull(col2, 0), left(col4, 22) from t1;
> ```

## Create Index Description

The `create index` statement creates an index on an existing base table. The index contains the columns specified. Any number of indexes can be created for a table, but each index can contain no more than 32 columns. The contents of indexes are sorted in ascending order by default.

Indexes can improve query processing. To obtain the greatest benefit, create indexes that contain all of the columns that are generally queried. The index must be keyed on a subset of those columns.

By default, the index is keyed on the columns in the column list, in the order they are specified. If the key option is specified, the index is keyed on the columns specified in the key list, in the order specified. For example, if you issue the statement:

```sql
create index nameidx on employee
        (last, first, phone);
```

you create an index named `nameidx` on the `employee` table that is keyed on the columns `last`, `first`, and `phone` in that order.

However, if you issue the statement:

```sql
create index nameidx on employee
(last, first, phone)
       with key = (last, first);
```

the index is keyed only on the two columns, `last` and `first`.

The columns specified in the key column list must be a subset of the columns in the main column list. A long varchar column cannot be specified as part of a key.

## Index Storage Structure

By default, indexes are created with an ISAM storage structure. There are two methods to override this default:

- To specify the default index storage structure for indexes created during the session, use the `-n` flag when issuing the command that opens the session (`connect`). For more information about this flag, see the Ingres System Administrator Guide.
- To override the session default when creating an index, specify the desired storage structure using the structure option when issuing the `create index` statement.

To specify whether the index is to be compressed, use the `with [no]compression` clause. By default, indexes are not compressed. If `with compression` is specified, the structure clause must be specified. An `RTREE` index cannot be compressed. To change the storage structure of an index, use the modify statement. For more information about table storage structures, see Modify Statement.

## Unique Indexes

To prevent the index from accepting duplicate values in key fields, specify the `unique` option. If the base table on which the index is being created has duplicate values for the key fields of the index, the `create index` statement fails. Similarly, if an `insert` or `update` is attempted that violates the uniqueness constraint of an index created on the table, the `insert` or `update` fails. This is true for an `update` statement that updates multiple rows: the `update` statement fails when it attempts to write a row that violates the uniqueness constraint.

## Effect of the `unique_scope` Option on Updates

The `unique_scope` option can affect the outcome of an update. For example, suppose you create an index on the employee numbers in an employee table, and the table contains employee numbers in sequence from 1 to 1000. If you issue an update statement that increments all employee numbers by 1, uniqueness is checked according to the `unique_scope` option as follows:

- **unique_scope = ROW** - Employee number 1 is incremented to 2. The row is checked for uniqueness—of course, employee number 2 already exists. **Result:** the update fails.
- **unique_scope = STATEMENT** - Employees 1 through 1000 are incremented before uniqueness is checked. All employee numbers remain unique. **Result:** the update succeeds.

## Index Location

`location_name` refers to the areas where the new index is created. The `location_names` must be defined on the system, and the database must have been extended to the corresponding areas. If no `location_name` is specified, the index is created in the default database area. If multiple `location_names` are specified, the index is physically partitioned across the locations. For more information about creating locations and extending databases, see the Ingres Database Administrator Guide.

## Create Index Permissions

To create an index, you must be the owner of a table. Users cannot update indexes directly. When a table is changed, the DBMS Server updates indexes as required. To create indexes on system tables, the effective user of the session must be `$ingres`.

## Create Index Locking

Creating an index on a table requires an exclusive lock on the table. This lock prevents other sessions, even those using the `readlock=NOLOCK` option, from accessing the table until `create index` completes and the transaction containing it is completed.

## Create Index Related Statements

- Create Table Statement
- Drop Statement
- Modify Statement

## Examples

### Example 1
Create an index for the columns, `ename` and `age`, on the `employee` table. The index is recreated when
