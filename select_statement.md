 of constants in the search condition. For example:
where dept = :var and name = 'Jones' and
     age > :age
You can also place the entire search condition in a varchar variable. For example:
qual = 'dept = ''mktg'' and age > 10 and name =
     ''Jones''';
...
select...
from...
where :qual;
To build up a search condition from the contents of existing varchar and numeric variables, you could do something like this:
qual = 'dept = ''mktg'' and age > ' + varchar(age) + ' and name =
          ''' + ename + '''';
If you place the entire search condition in a varchar variable, you cannot use the repeated keyword in the select statement.
Order By Clause
The order by clause lets you specify how the returned rows are ordered. Its syntax is:
order by orderspecification{, orderspecification}
where orderspecification is:
name |number [sortorder]
You can use either the names or numbers to specify the order, but not both. Additionally, if you use a dynamic name to specify either name or sortorder, you cannot use the repeated option in the select statement.
The name must be taken from a simple_variable, resultname, or dbexpression (if it specifies a database column and simple_variable and resultname are omitted) in the select's target list. If the name in the select target list is expressed with dot notation, use the last component of that notation as the name in the order by clause. For example:
select empname as name,
empage as e.age,
empsalary as emptab[3].salary
from emptable
order by age, salary, name
The number is the ordinal position of the name in the select target list. For example, consider the following select statement:
select empname as e.name
       department as e.dept
from emptable
order by 2, 1
begin...
In this statement, e.name is in ordinal position one in the target list and e.dept is in ordinal position two. In the order by clause, we are directing the DBMS to order the returned rows by the variable name in position two, that is, e.dept (the department) and then by the variable name in position one, e.name.
If you include more than one name or number, the DBMS orders the rows on the basis of the first item, the second item, and so on. For example, assume that your application has the following select statement:
select ename as employee_name, dept as dept,
     salary as salary, eage as age
from employee
order by dept, employee_name, salary asc;
If the returned rows are displayed in a table field, the user sees them sorted in this manner:
Dept
Name
Salary
1
Adams, Harold
35,000
1
James, Clarice
36,000
1
James, Donna
27,000
2
Gordon, Gerard
37,000
2
Sevarino, Juan
31,000
The rows are sorted first by department, then within each department by name, and within each name (if two are the same) by salary.
The sortorder determines how each item is sorted within itself. You can specify an ascending (asc) or descending (desc) sort order for each item. If you do not specify sortorder, the default is ascending.
If you are sorting by names that are the same, you must use numbers. For example, numbers must be used in the following order by clause:
select empname as e.name,
mgrname as m.name
from emptable
order by 1, 2
Using a Select Loop
A select loop is one or more operations that the program performs on each row of values returned by a select statement. Because select loops provide better performance than cursors, you should use select loops whenever the operations that you want to perform on the returned rows are completed within a single event block.
The begin and end keywords define the statement block that is the select loop. You can use any legal 4GL statement in the statement list.
The system variable IIrowcount has an undefined value during the execution of a select loop unless you execute a query statement as part of the loop. In such cases, the value of IIrowcount is defined for that statement. After the select loop completes, the value in IIrowcount reflects the number of rows processed by the loop.
For more information about using select loops and cursors, see the Programming Guide.
Examples--Select Statement
Select information about an employee based on the value in the empnum field. Then use the commit statement to end the transaction:
select last as lname, first as fname
from personnel
where empnum = :empnum;
commit;
Select rows for all employees with an employee number greater than 100, then use the commit statement to end the transaction. The table name is held in a variable:
tbl = 'employee';
select last as lname, first as fname,
employee as empnum
from :tbl
where number >100;
commit;
Read into the emptable array all projects for the employee whose number is currently displayed in the empnum field, then use the commit statement to end the transaction:
i = 1;
repeated select project as emptable[i].project,
hours as emptable[i].hours
from projects
where empnum = :empnum
begin
     i = i + 1;
end
commit;
Retrieve the name and salary and set a variable to indicate the status of the salary:
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
