# Fetch Statement

This statement retrieves a row from an open cursor and loads it into 4GL variables.

## Syntax

```
fetch cursor_variable into variable_list;
```

You can use the `fetch` statement to:
- Move the cursor forward one row in the select statement's result table
- Put the values from the specified columns into the specified variables
- Set the `State` and `RowCount` attributes of the `CursorObject` class

After a successful fetch, you can process the values in the variables any way you want. To display the cursor data in the window, you can fetch it into the variables that are associated with the fields on your form.

The retrieved row becomes the current row, that is, the row affected by the next `update cursor` or `delete cursor` statement for this cursor.

You can use a simple list of variables whenever you:
- Retrieve all of the values in the result row, in the order specified by the select statement
- Retrieve a subset of the values that begins with the first value retrieved by the select and continues in the order listed in the select (stopping before all are retrieved)

You must specify the column names whenever you fetch the column values out of order, that is, not in the same order in which they were specified in the select statement. For example, if you select columns A, B, C, and D and then fetch columns B and D, you must specify the column names. When you specify the column names, you can fetch the columns in any order. If you retrieved values into `resultnames` in your select statement, use these `resultnames` as the `columnnames` in the `variable_list` of your `fetch` statement.

The `fetch` statement can contain fewer variables than there are select expressions in the `open` statement, but it cannot contain more. Whether you specify the column names or not, the data types of the variables and the values that are assigned to them must be compatible. OpenROAD does not check the compatibility of the variables and column names until runtime. If there is a mismatch, OpenROAD reports it in the `iierrornumber` system variable, although the row is still fetched and becomes the current row.

If the `fetch` statement is successful, 4GL sets the value of the `iirowcount` system variable to one. If the `fetch` statement is unsuccessful or there are no more rows to fetch, 4GL sets `iirowcount` to zero and does not change the variables. Additionally, if a `fetch` statement finds no rows to fetch, it sets the `State` attribute to `CS_NO_MORE_ROWS`. Consequently, you can check the `State` attribute after each fetch to determine if there are more rows to fetch.

To find out how many rows have been successfully fetched, check the `RowCount` attribute of the `CursorObject` class. OpenROAD sets this attribute to zero whenever the cursor object is opened and increments it after each successful fetch.

You must issue any `fetch` statements for a cursor in the same DBMS session in which you opened the cursor.

For more information and examples of specifying columns in a `fetch` statement, and for a description of how to use cursors in OpenROAD, see the Programming Guide.

## Parameters

This statement has the following parameters:

### cursor_variable
Specifies a reference variable that points to an object of type `CursorObject`. This cursor must be open.

### variable_list
Specifies the variables into which you load your data. You can format this list as:
```
:variable{, :variable}
```
or
```
:variable = columnname{, :variable = columnname}
```
You cannot mix the two formats.

## Examples

Check the `State` attribute of the `CursorObject` to determine whether there are remaining rows:
```sql
fetch emp_cursor into :emp_name = name, :emp_age = age;
if emp_cursor.State = CS_CURRENT then
          /* A new row has been fetched. */
...
elseif emp_cursor.State = CS_NO_MORE_ROWS then
          /* All rows have been fetched. */
...
else
          /* There is an error. */
endif;
```

Check the `RowCount` attribute of the `CursorObject` to check the number of rows that have been retrieved:
```sql
/* Fetches no more than 5 rows from CursorObject */
if emp_cursor.RowCount <= 5 then
     fetch emp_cursor into :emp_name = name, :emp_age = age;
else
     close emp_cursor;
endif;
```
