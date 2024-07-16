Create View Statement
The create view statement defines a virtual table.
This statement has the following syntax:
create view view_name
               [(column_name {, column_name})]
               as select_stmt
              [with check option];
Parameters--Create View Statement
This statement has the following parameters:
view_name
Defines the name of the view. It must be a valid object name.
column_name
Specifies the name of a column in the view.
select_stmt
Specifies a select statement, as described in the select statement description in this chapter.
with check option (see With Check Option Clause)
Prevents an insert or update to a view that creates a row that does not comply with the view definition.
Create View Description
The create view statement uses a select statement to define the contents of a virtual table. The view definition is stored in the system catalogs. When the view is used in a statement, the statement operates on the underlying tables. When a table or view used in the definition of a view is dropped, the view is also dropped.
Data can be selected from a view the same way data is selected from a base table. However, updates, inserts, and deletes on views are subject to several restrictions. Updates, inserts, and deletes are allowed only if the view meets all the following conditions:
•The view is based on a single updatable table or view.
•All columns see columns in the base table or view (no aggregate functions or derived columns are allowed).
•The select statement omits distinct, group by, union, and having clauses.
Inserts are not allowed if a mandatory (not null not default) column in a base table is omitted from the view.
A maximum of 1024 columns can be specified for a view.
Note:  This statement has additional considerations when used in a distributed environment. For more information, see the Ingres Star User Guide.
With Check Option Clause
The with check option clause prevents you from executing an insert or update to a view that creates a row that does not comply with the view definition (the qualification specified in the where clause).
For example, if the following view is defined with check option:
create view myview
    as select *
    from mytable
    where mycolumn = 10
    with check option;
And the following update is attempted:
update myview set mycolumn = 5;
The update to the mycolumn column is rolled back, because the updated rows fail the mycolumn = 10 qualification specified in the view definition. If the with check option is omitted, any row in the view can be updated, even if the update results in a row that is no longer a part of the view.
The with check option is valid only for updatable views. The with check option clause cannot be specified if the underlying base table is used in a subselect in the select statement that defines the view. You cannot update or insert into a view defined on top of a view specified with with check option if the resulting rows violate the qualification of the underlying view.
Create View Permissions
You must have all privileges required to execute the select statements that define the view.
Create View Locking
The create view statement requires an exclusive lock on the view's base tables.
Create View Related Statements
Drop Statement
Examples--Create View Statement
The following are create view statement examples:
1.Define a view of employee data including names, salaries, and name of the manager.
create view empdpt (ename, sal, dname)
    as select employee.name, employee.salary,
    dept.name
    from employee, dept
    where employee.mgr = dept.mgr;
2.Define a view that uses aggregate functions to display the number of open orders and the average amount of the open orders for sales representative that has orders on file. This view is not updatable (because it uses aggregate functions).
create view order_statistics
    (sales_rep, order_count, average_amt)
      as select salesrep, count(*), avg(ord_total)
      from open_orders
      group by sales_rep;
3.Define an updatable view showing the employees in the southern division. Specify check option to prevent any update that changes the region or adds an employee from another region.
create view southern_emps
    as select * from employee
    where region = 'South'
    with check option;
