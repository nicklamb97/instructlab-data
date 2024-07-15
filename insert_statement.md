Insert Statement
This statement inserts rows into a database table.
This statement has the following syntax:
[repeated] insert into tablename [(columnname {, })]
          values(expression{, expression}) | subselect;
The insert statement inserts new rows into the specified database table.
When you specify columnname, you must specify any column that is defined “not null not default.” If you do not specify such columns, you receive an error and the statement is not executed. Columns that are nullable or defaultable can be specified or not, as appropriate. If unspecified, they receive default values. (OpenROAD default values are null if the column is nullable, or blank for character data type columns and zero for numeric columns.)
The repeated keyword directs the database to encode and save the query execution plan, providing performance benefits if you intend to execute this statement more than once. Valid only for the Ingres DBMS.
Do not use the repeated keyword if your statement uses a dynamic name for the table name, a column name, or in the subselect clause.
For more information about the insert statement, see the Programming Guide.
Parameters--Insert Statement
This statement has the following parameters:
tablename
Specifies the name of the database table into which rows are to be inserted. This is a dynamic name.
columnname
Specifies the name of a column within the database table that receives the value. This is a dynamic name.
expression
Specifies an SQL expression that must evaluate to the appropriate data type.
This expression has the restriction that it must not contain references to database column names.
subselect
Specifies a select statement that serves as the source of values to be inserted
Values Clause and Subselect
To specify the values to insert, use the values clause or a subselect.
•In the values clause, expression represents the value that you are inserting into the database.
•When you use a subselect, the values to insert are retrieved by the select statement. For more information about subselects, see Select Statement.
You can omit the list of database columns if the inserted values come from a subselect and the column types in the subselect match the column types in the table in the order specified when the table was created.
Examples--Insert Statement
Insert the values of projname and enddate into the projects table and then commit the changes:
insert into projects(name, duedate)
     values(:projname, :enddate);
commit;
Insert a computed value into the personnel table:
repeated insert into personnel(name, sal)
     values(:name, :salary*1.1);
Insert data from the contractor table into the personnel table, according to qualifying criteria, then commit the changes:
repeated insert into personnel(name, sal)
     select name, salary from contractor
          where name = :f_name and age = :f_age;
commit;
Take the rows from the emptable array and insert them into the database:
i = 1;
while i <= emptable.LastRow do
     repeated insert into employee(name, age, salary)
          values(:emptable[i].name,
               :emptable[i].age,
               :emptable[i].salary);
     i = i + 1;
endwhile;
