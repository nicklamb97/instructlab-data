
# Update Statement

This statement updates the values of the columns in a database table.

## Syntax

### Non-cursor version:
```
[repeated] update tablename [corrname]
     [from fromclause]
     set columnname = dbexpression {, columnname =
               dbexpression}
     [where searchcondition];
```

### Cursor version:
```
update tablename set columnname = dbexpression
     {, columnname = dbexpression} where current of
          cursor_variable;
```

where `fromclause` has the following alternative syntaxes:
```
from tablename [corrname]{, tablename [corrname]}
from :fromvariable
```

The update statement modifies the values in columns in a database table.

The `tablename` identifies the table that you want to update and must match the table name specified in the open statement for the specified cursor.

Each column that you include in the update statement must also have been specified in the for update clause of the open statement that opened the cursor.

The optional `corrname` defines a correlation name for the table. Correlation names provide a shorthand way to refer to tables in a query.

The optional `repeated` keyword directs the DBMS to encode and save the query execution plan for the statement. This is a performance enhancement if you intend to execute this statement more than once.

> Note: Do not use the repeated keyword if you place the entire where clause, searchcondition, in a single variable or if a table name or column name is represented by a dynamic name.

## Parameters--Update Statement

This statement has the following parameters:

### tablename
Specifies the name of the database table that you want to update. This is a dynamic name.

### corrname
Specifies the correlation name for the table. This is a dynamic name.
This parameter is used only in the non-cursor version.

### columnname
Specifies the name of a particular column to be updated in the table. This is a dynamic name.

### dbexpression
Specifies a database expression

### searchcondition
Specifies a logical database expression of conditions that must be satisfied by all rows selected. You can use simple variables (preceded by colons) in a search condition wherever you can use a literal. Alternatively, you can place the entire search condition in a single varchar variable, and specify the search condition as the name of this variable (proceeded by a colon).
This parameter is only used in the non-cursor version.

### cursor_variable
Specifies the reference variable that points to the CursorObject for which the statement is issued. Used only in the cursor version.

### fromvariable
Specifies the entire from clause as a single varchar variable, preeded by a colon. This is a dynamic name.

## Set Clause

The set clause specifies the columns in the table that you want to update.

## Where Clause

If you do not include a where clause, OpenROAD updates all of the rows in the table. If you include a where clause, OpenROAD updates only the rows in the table that satisfy searchcondition in the where clause.

The "where current of" clause directs OpenROAD to update the row on which the specified cursor is positioned. The cursor_variable must point to a CursorObject that you opened using a for update clause.

## Update Mode

The update mode of the cursor, specified when you opened the cursor, affects how you can use the update statement. You can open a cursor for deferred or direct update.

### Deferred Mode

With deferred mode cursors, you can perform only one operation against each row. This operation can be an update or a delete but not both. Similarly, you cannot use the repeated option with a deferred update cursor. Cursors that are opened for direct update do not have these restrictions. However, if the cursor is a direct update cursor and you perform two cursor updates that are not separated by a fetch statement, you can update the same row twice.

After a successful update, OpenROAD sets the IIrowcount system variable to one. The cursor remains pointing to the same row (the State attribute of the CursorObject remains CS_CURRENT) and subsequent update or delete statements for the cursor take effect on the same row. If the update is not successful, OpenROAD sets IIrowcount to -1 or 0.

If the cursor is not pointing to a row, 4GL generates a runtime error message indicating that you must execute a fetch statement. If the row the cursor is pointing to has been deleted from the underlying database table (as the result, for example, of a non-cursor delete), no row is updated.

It is not necessary to commit after each cursor update. All the changes that you make are committed when you close the cursor. (Do not update the current row of a cursor, commit the change, and then continue in a loop in order to repeat the process. This process fails because the first commit statement closes the cursor.)

You must issue any update statements for a cursor in the same session in which you opened the cursor.

For more information about the update statement, see the Programming Guide.

## Examples--Update Statement

Update rows in the projects table with values from the current frame:
```
repeated update projects
     set hours = :hours, duedate = :enddate
     where name = :name;
commit;
```

Update the personnel table with a computed value:
```
update personnel
     set sal = :salary * 1.1
     where empno = :empno;
commit;
```

Update the salaries of all employees in job category, acc, using the value for a standard raise in the table dept:
```
update employee e from dept d
     set salary = d.std_raise * e.salary
     where e.jobcat = 'acc' and d.dname = e.dname;
commit;
```

Update the vendor address based on the attributes of the addr reference variable:
```
update vendor
     set city = :addr.city, state = :addr.state
     where vendorno = :vno;
commit;
```

Update the row pointed to by the open cursor, emp_cursor:
```
update emptable
     set name = :namefield, age = :agefield
     where current of emp_cursor;
commit;
```
