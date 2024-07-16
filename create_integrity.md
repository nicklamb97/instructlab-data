Create Integrity Statement
The create integrity statement creates an integrity constraint for the specified base table.
This statement has the following syntax:
create integrity on table_name [corr_name]
              is search_condition;
Parameters--Create Integrity Statement
This statement has the following parameters:
table_name
Specifies the table for which the constraint is defined.
corr name
Specifies a correlation name for the table for use in the search condition.
search_condition
Defines the actual constraint. For example, if you want to create a constraint on the employee table so that no employee can have a salary of greater than $75,000, issue the following statement:
create integrity on employee is salary <= 75000;
The search condition must reference only the table on which the integrity constraint is defined, and cannot contain a subselect or any aggregate (set) functions.
At the time the create integrity statement is executed, the search condition must be true for every row in the table, or the DBMS Server issues an error and aborts the statement. If the search condition is defined on a column that contains nulls, the statement fails unless the is null predicate is specified in the statement.
After the constraint is defined, all updates to the table must satisfy the specified search condition. Integrity constraints that are violated are not specifically reported: updates and inserts that violate any integrity constraints are simply not performed.
Create Integrity Locking
The create integrity statement takes an exclusive lock on the specified table.
Create Integrity Performance
The time required to execute the create integrity statement varies with the size of the table, because the DBMS Server must check the specified base table to ensure that each row initially conforms to the new integrity constraint.
Permissions: Own the table
You must own the table.
Create Integrity Related Statements
Drop Integrity Statement
Examples--Create Integrity Statement
The following are create integrity statement examples:
1.Make sure that the salaries of all employees are no less than 6000.
create integrity on employee is salary >= 6000;
2.In an OpenROAD application, define an integrity using a variable.
create integrity on employee
    is sal < :sal_limit;
