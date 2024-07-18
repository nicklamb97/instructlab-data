# Delete Statement

The `delete` statement removes rows from a database table.

## Syntax

- **Non-cursor version:**
  ```
  [repeated] delete from tablename [corrname]
       [where searchcondition];
  ```

- **Cursor version:**
  ```
  delete from tablename where current of cursor_variable;
  ```

### Non-cursor Version

The non-cursor version of the `delete` statement deletes selected rows from a table based on the specified conditions in the `where` clause. If the `where` clause is omitted, all rows in the table are deleted.

The `repeated` keyword optimizes query performance by saving and reusing the query execution plan for subsequent executions, applicable only to Ingres database systems.

### Cursor Version

The cursor version deletes the row that is currently pointed to by the specified cursor. The cursor must be opened and fetched before issuing a `delete` statement.

Before using a cursor delete statement:
- Ensure the cursor is opened and, if necessary, fetched.
- For direct update cursors, deletions are immediate; for deferred update cursors, deletions take effect when the cursor is closed.

After a successful cursor delete statement, the CursorObject's State attribute is set to `CS_NOCURRENT`, indicating the cursor is positioned after the deleted row but before the next row.

## Parameters

- **tablename**: Specifies the name of the table from which rows are deleted.
- **corrname**: Correlation name of the table used in the non-cursor version.
- **searchcondition**: Logical expression defining the conditions for row selection.
- **cursor_variable**: Reference variable pointing to the CursorObject used in the cursor version.

## Examples

### Example 1: Non-cursor Delete

Delete all rows in the `personnel` table where `empno` matches the current frame, using the `repeated` option, then commit the changes:
```sql
repeated delete from personnel
          where personnel.empno = :empno;
commit;
```

### Example 2: Non-cursor Delete

Delete all rows in the `personnel` table:
```sql
delete from personnel;
```

### Example 3: Non-cursor Delete with Variables

Delete rows from a dynamically specified table (`tablename`) and condition (`whereclause`):
```sql
whereclause = 'empno = 12';
tablename = 'personnel';
delete from :tablename where :whereclause;
```

### Example 4: Cursor Delete

Delete the row pointed to by the `emp_cursor` cursor object:
```sql
delete from emp where current of emp_cursor;
if iirowcount = 0 then
message 'Delete failed';
endif;
```

*Note:* Replace `emp` and `emp_cursor` with appropriate table and cursor names.

*Note:* To delete rows from a table specified by a dynamic name, use `delete from :tablename`.
