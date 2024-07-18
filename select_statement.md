# Select Statement

This statement retrieves rows from a database table.

This statement has the following syntax:

```
[repeated] subselect
{union [all] subselect}
[order by orderclause
[beginendblock];
```

where subselect has the following syntax:

```
select [all | distinct]
          resultexpression{, resultexpression}
          from fromclause
          [where searchcondition]
          [group by groupclause]
          [having searchcondition]
```

and fromclause has the following alternative syntaxes:

```
from tablename [corrname]{, tablename [corrname]}
from :fromvariable
```

and groupclause has the following alternative syntaxes:

```
[group by columnname{, columnname}]
[group by :groupvariable
```

and orderclause has the following alternative syntaxes:

```
[order by orderspecification{, orderspecification}]
[order by :ordervariable
```

You can use the select statement in two ways:
- To retrieve one row of data (called a "singleton select")
- To retrieve multiple rows into a result table

If you retrieve multiple rows, you can use either a select loop or a cursor to process them. Use the keyword, distinct, to eliminate duplicate rows from the result table.

The optional repeated keyword directs the database to encode and save the query execution plan. This feature is a performance benefit if you plan to execute this query more than once in the application. Do not use the repeated option if you use dynamic names for tables or columns, or use a single variable for the searchcondition specification in the statement.

When you want to retrieve only one row or want to use a select loop to process multiple rows, use either of the following formats for the value of resultexpression:

```
simple_variable = dbexpression
```

or

```
dbexpression as simple_variable
```

If you intend to use a cursor to process the rows, the select statement is part of an open cursor statement. Alternatively, if you are coding the select as the subselect of an insert statement, use one of the following formats for the value of resultexpression:

```
resultname = dbexpression
dbexpression as resultname
dbexpression
*
corrname.*
```

Note: The asterisk selects values from all columns.

If you are using a cursor to process the selected rows, you must use the fetch statement to access the returned values. If you retrieve values into a resultname, use this name as the value of columnname in the variable list of your fetch statement.

For more information about using a select statement with cursors and the fetch statement, see the Programming Guide.

You can use the optional order by clause to sort rows alphabetically or numerically, in ascending or descending order. The description of the order by clause that follows provides the details about using this clause.

The begin and end keywords define a statement block. Include this statement block when you want to process the rows with a select loop. OpenROAD performs the operations defined in the statementlist on the values in each row returned by the select statement as the row is returned. Do not use this block if the select statement is part of an open cursor or insert statement. You may use { } instead of begin and end.

If you are selecting into field variables and the select statement returns no rows, OpenROAD does not clear the fields (simple fields or table fields). The fields retain whatever values they displayed before the select statement was executed.

## Parameters--Select Statement

This statement has the following parameters:

- **dbexpression**: Specifies a database expression (also known as an SQL expression) that cannot include 4GL reference or array variables, 4GL procedures, or the 4GL field function
- **simple_variable**: Specifies the simple variable that receives the value from a specified dbexpression. The data types of the receiving object and dbexpression must be compatible.
- **beginendblock**: See BeginEndBlock.
- **resultname**: Identifies the column name (for example, for subsequent fetch statements) in which a dbexpression is placed
- **tablename**: Specifies the name of the table containing the columns from which data is to be selected. This is a dynamic name.
- **corrname**: Specifies the correlation name that you give to a table. If this parameter is omitted, the table name is used as the correlation name.
- **searchcondition**: Specifies a logical database expression of conditions that must be satisfied by all rows selected. You can use simple variables (preceded by colons) in a search condition wherever you can use a literal. Alternatively, you can place the entire search condition in a single varchar variable, and specify the search condition as the name of this variable (preceded by a colon).
- **columnname**: Specifies the name of a column in the database table from which data is selected and by which rows are grouped. This name is a dynamic name.
- **orderspecification**: Specifies a column or list of columns on which to sort. For more information, see Order By Clause.
- **fromvariable**: Specifies the entire from clause as a single varchar variable, preceded by a colon. This is a dynamic name.
- **groupvariable**: Specifies the entire group by clause as a single varchar variable, preceded by a colon. This is a dynamic name.
- **ordervariable**: Specifies the entire order by clause as a single varchar variable, preceded by a colon. This is a dynamic name.

## Union Clause

The union clause combines the results of a number of select statements. The general syntax is:

```
subselect
union subselect {union subselect}
```

You can enclose a subselect in parentheses to make it easier to read unioned select statements.

OpenROAD uses the column names in the first subselect's column list to build the correspondence to the frame variables. Consequently, in the column list for the second and subsequent selects, you cannot use the following formats:

```
:var = column, ...
column as var, ...
```

You can join any number of subselects with the union clause. However, all the rows returned by a unioned select statement must be identical to each other in type. For example, the following select statement selects the names and numbers of people from different organizations:

```
select ename as name, enumber as number
from employee
union
select dname, dnumber
from directors
where dnumber <= 100;
```

The previous statement is valid because each subselect is returning an employee name and number. You could not union a select that returns an employee name and number with a select statement that returns, for example, an employee name and address because the two select statements are not returning columns of the same type and format.

## Outer Joins

You can combine data from two or more tables to produce an intermediate results table using an outer join.

Outer joins specified in the from clause are not the same as joins specified in the where clause: the from clause specifies sources of data, while the where clause specifies restrictions to be applied to the sources of data to produce the results table.

Outer joins are specified in the from clause using the following syntax:

```
source join_type join source
          on search_condition
```

- **source**: Specifies the table, view, or outer join where the data for the left or right side of the join originates
- **join_type**: Specifies an inner, left, right, or full outer join. Default: inner
- **search_condition**: Specifies a valid restriction, subject to the rules for the where clause. The search condition must not include aggregate functions or subselects.

You may think of an outer join is as the union of two select statements: the first query returns rows that fulfill the join condition, and the second returns nulls for rows that do not.

There are three types of outer joins:

- **Left outer join**: Returns all values from the left source
- **Right outer join**: Returns all values from the right source
- **Full outer join**: Returns all values from both sources

Right and left joins are symmetrical: (table1 right-join table2) returns the same results as (table2 left-join table1).

By default, joins are evaluated left to right. To override the default order of evaluation, use parentheses.

A source can itself be an outer join, and you can join the results of joins with the results of other joins, as illustrated in the following pseudo code:

```
(A join B) join (C join D)
```

How you place restrictions is important in obtaining correct results. For example:

```
A join B on cond1 and cond2
```

does not return the same results as:

```
A join B on cond1 where cond2
```

In the first example, the restriction determines which rows in the join result table will be assigned null values; in the second example, the restriction determines which rows will be omitted from the result table.

The following example uses an outer join in the from clause to display all employees with the name of their department, if any:

```
select e.ename, d.dname from
     (employees e left join departments d
          on e.edept = d.ddept);
```

## Where Clause

The where clause tells the database what rows you want to retrieve. The value of searchcondition is a logical (Boolean) database expression that defines some criteria for row selection

When you include this clause, the select statement returns only those rows that fit the criteria defined in the search condition. If you do not include a where clause, the select statement returns as many rows as there are rows in the table. (If the select statement unions two or more selects, then it returns all the rows from the table in each select. Similarly, if a select joins two or more tables, then it returns a Cartesian product if there is no where clause.) The where clause lets you specify which rows you want returned by the select statement.

The syntax of the where clause is:

```
where searchcondition
```

**searchcondition**: Specifies a logical (Boolean) expression. The expression must compare a column value (or an aggregate of column values) to some expression, such as a constant value or a field in the current form.

For example:

```
select name as name, jobtitle as jobtitle
from employee
where salary >= :sallimit;
```

This statement selects into the frame variables the name and job title of the first employee returned whose salary is equal to or greater than the value entered in the sallimit field.

You can use simple variables in place of constants in the search condition. For example:

```
where dept = :var and name = 'Jones' and
     age > :age
```

You can also place the entire search condition in a varchar variable. For example:

```
qual = 'dept = ''mktg'' and age > 10 and name =
     ''Jones''';
...
select...
from...
where :qual;
```

To build up a search condition from the contents of existing varchar and numeric variables, you could do something like this:

```
qual = 'dept = ''mktg'' and age > ' + varchar(age) + ' and name =
          ''' + ename + '''';
```

If you place the entire search condition in a varchar variable, you cannot use the repeated keyword in the select statement.

## Order By Clause

The order by clause lets you specify how the returned rows are ordered. Its syntax is:

```
order by orderspecification{, orderspecification}
```

where orderspecification is:

```
name |number [sortorder]
```

You can use either the names or numbers to specify the order, but not both. Additionally, if you use a dynamic name to specify either name or sortorder, you cannot use the repeated option in the select statement.

The name must be taken from a simple_variable, resultname, or dbexpression (if it specifies a database column and simple_variable and resultname are omitted) in the select's target list. If the name in the select target list is expressed with dot notation, use the last component of that notation as the name in the order by clause. For example:

```
select empname as name,
empage as e.age,
empsalary as emptab[3].salary
from emptable
order by age, salary, name
```

The number is the ordinal position of the name in the select target list. For example, consider the following select statement:

```
select empname as e.name
       department as e.dept
from emptable
order by 2, 1
begin...
```

In this statement, e.name is in ordinal position one in the target list and e.dept is in ordinal position two. In the order by clause, we are directing the DBMS to order the returned rows by the variable name in position two, that is, e.dept (the department) and then by the variable name in position one, e.name.

If you include more than one name or number, the DBMS orders the rows on the basis of the first item, the second item, and so on. For example, assume that your application has the following select statement:

```
select ename as employee_name, dept as dept,
     salary as salary, eage as age
from employee
order by dept, employee_name, salary asc;
```

If the returned rows are displayed in a table field, the user sees them sorted in this manner:

| Dept | Name           | Salary |
|------|----------------|--------|
| 1    | Adams, Harold  | 35,000 |
| 1    | James, Clarice | 36,000 |
| 1    | James, Donna   | 27,000 |
| 2    | Gordon, Gerard | 37,000 |
| 2    | Sevarino, Juan | 31,000 |

The rows are sorted first by department, then within each department by name, and within each name (if two are the same) by salary.

The sortorder determines how each item is sorted within itself. You can specify an ascending (asc) or descending (desc) sort order for each item. If you do not specify sortorder, the default is ascending.

If you are sorting by names that are the same, you must use numbers. For example, numbers must be used in the following order by clause:

```
select empname as e.name,
mgrname as m.name
from emptable
order by 1, 2
```

## Using a Select Loop

A select loop is one or more operations that the program performs on each row of values returned by a select statement. Because select loops provide better performance than cursors, you should use select loops whenever the operations that you want to perform on the returned rows are completed within a single event block.

The begin and end keywords define the statement block that is the select loop. You can use any legal 4GL statement in the statement list.

The system variable IIrowcount has an undefined value during the execution of a select loop unless you execute a query statement as part of the loop. In such cases, the value of IIrowcount is defined for that statement. After the select loop completes, the value in IIrowcount reflects the number of rows processed by the loop.

For more information about using select loops and cursors, see the Programming Guide.

## Examples--Select Statement

Select information about an employee based on the value in the empnum field. Then use the commit statement to end the transaction:

```
select last as lname, first as fname
from personnel
where empnum = :empnum;
commit;
```

Select rows for all employees with an employee number greater than 100, then use the commit statement to end the transaction. The table name is held in a variable:

```
tbl = 'employee';
select last as lname, first as fname,
employee as empnum
from :tbl
where number >100;
commit;
```

Read into the emptable array all projects for the employee whose number is currently displayed in the empnum field, then use the commit statement to end the transaction:

```
i = 1;
repeated select project as emptable[i].project,
hours as emptable[i].hours
from projects
where empnum = :empnum
begin
     i = i + 1;
end
commit;
```

Retrieve the name and salary and set a variable to indicate the status of the salary:

```
i = 1;
select name as name, salary as salary
from emptable
begin
     if salary > 100000 then
          overpaid = TRUE;
     else
          overpaid = FALSE;
     endif;
     emptable[i].name = name;
     emptable[i].salary = salary;
     emptable[i].overpaid = overpaid;
     i = i + 1;
end
```
