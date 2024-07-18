# Create View Statement

The `create view` statement defines a virtual table based on a `select` statement.

## Syntax

```sql
create view view_name
               [(column_name {, column_name})]
               as select_stmt
               [with check option];
```

## Parameters

### view_name

Defines the name of the view. It must be a valid object name.

### column_name

Specifies the name of a column in the view.

### select_stmt

Specifies a `select` statement that defines the view's contents.

### with check option

Prevents an insert or update to a view that creates a row not complying with the view definition.

## Description

The `create view` statement uses a `select` statement to define a virtual table (`view`). The view definition is stored in system catalogs. Operations on the view affect the underlying tables. Dropping a table or view used in a view's definition results in dropping the view.

Data retrieval from a view is akin to that from a base table. However, updates, inserts, and deletes on views have constraints:

- The view must be based on a single updatable table or view.
- All columns must directly map to columns in the base table or view.
- The `select` statement cannot include `distinct`, `group by`, `union`, or `having` clauses.
- Mandatory columns in base tables (not null, not default) cannot be omitted from the view.
- Views can have a maximum of 1024 columns.

*Note:* Distributed environments have additional considerations.

## With Check Option Clause

The `with check option` clause prevents inserting or updating rows in a view that do not meet the view's `where` clause qualification.

For example, with a view `myview`:

```sql
create view myview
    as select *
    from mytable
    where mycolumn = 10
    with check option;
```

An attempt to update `mycolumn`:

```sql
update myview set mycolumn = 5;
```

will fail because the update violates the `mycolumn = 10` qualification.

## Permissions

Users need privileges to execute `select` statements used in view definitions.

## Locking

The `create view` statement requires an exclusive lock on the view's base tables.

## Related Statements

- **Drop Statement**

## Examples

### Example 1

Define a view of employee data including names, salaries, and manager names:

```sql
create view empdpt (ename, sal, dname)
    as select employee.name, employee.salary,
    dept.name
    from employee, dept
    where employee.mgr = dept.mgr;
```

### Example 2

Define a view using aggregate functions to show the number and average amount of open orders for sales representatives:

```sql
create view order_statistics
    (sales_rep, order_count, average_amt)
      as select salesrep, count(*), avg(ord_total)
      from open_orders
      group by sales_rep;
```

### Example 3

Define an updatable view showing employees in the southern division with check option:

```sql
create view southern_emps
    as select * from employee
    where region = 'South'
    with check option;
```
