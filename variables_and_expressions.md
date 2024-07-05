# Variables in Expressions

You can use simple variables alone or in expressions. In the following example, the age variable appears alone on the 
left side and is an element in the expression "age + 1" on the right of the assignment statement:

```
age = age + 1;
```

OpenROAD uses the current value of age to compute a new value that replaces the current value.

You can use the simple variables that are elements of reference or array variables in the same way, for example:

```
total = 0;
i = 1;
while i <= emptable.LastRow do
    total = total + emptable[i].salary;
    i = i + 1;
endwhile;
```

In this example, the value in the salary column from the emptable table field is added to the total field as part of 
a while loop.

# Procedures in Expressions

When a procedure returns a value, such as a return code, it functions as an expression. In the following syntax 
example, procname is the name of a procedure that returns a value:

```
returnval = procname() + 1;
```

When you call a procedure as part of an expression, the following rules apply:

- Do not use the callproc keyword.
- Include the parentheses even if you are not passing any parameters to the procedure.
- Explicitly name the procedure. You can use a variable for the name of a procedure if you assign it to a variable. 
  For example, the following assignment is legal:
  ```
  a = :varproc();
  ```
  However you cannot use operators when you are assigning a variable as the procedure name. For example, the 
  following expression is illegal and cannot be specified at runtime:
  ```
  a = :varproc() + 7;
  ```

Because procedures can return a value of any type, such as an object or an array, you can operate on the return value 
with any operation appropriate to the return value type. In the syntax example just described, procname is a 
procedure that returns a numeric type like integer or float, allowing you to use the return value in the addition.

If a procedure returns an object, the return value can be manipulated like any other object. That is, you can use the 
dot operator (.) to access individual attributes, or you can apply a method to that object.

Using the dot operator produces a variable of some kind (depending on the type of the attribute), so you can use the 
resulting variable wherever other variables can be used. For example, if the addr_proc procedure returns a variable 
of class Addr that has an attribute "City," the following expression is legal:

```
addr_proc().City = 'New York';
```

Because you can apply methods to the return value, and because methods can return values of any type, referencing can 
be nested on a procedure's return value, for example:

```
ret_framexec().ObjectSource.Duplicate().name
```

# Methods in Expressions

The 4GL statement, method invocation, lets you invoke a method as part of an expression. For more information about 
this statement, see Method Invocation Statement.

When a method returns a value, it can function as an expression. In the following syntax example, methodname is the 
name of a method that returns a value:

```
returnval = objectref.methodname() + 1;
```

When you call a method as part of an expression, you must explicitly name the method. You can use a variable for the 
name of a method if you assign the name to a varchar variable. For example, the following assignment is legal:

```
a = objectref.:varmeth();
```

However you cannot use operators when you are assigning a variable as the method name. For example, the following 
expression is illegal and cannot be specified at runtime:

```
a = objectref.:varmeth() + 7;
```

Because methods can return a value of any type, such as an object or an array, you can operate on the return value 
with any operation appropriate to the return value type. In this syntax example, methodname is a method that returns 
a numeric type like integer or float, allowing you to use the return value in the addition.

If a method returns an object, the return value can be manipulated like any other object. That is, you can use the 
dot operator (.) to access individual attributes, or you can apply a method to that object.

Using the dot operator gets a variable of some kind (depending on the type of the attribute), so you can use the 
resulting variable wherever other variables can be used. For example, if the addr_meth method returns a variable of 
class Addr that has an attribute of City, the following expression is legal:

```
objectref.Addr_meth().City = 'New York';
```

# Nulls in Expressions

Because a null cannot be compared to another value, the only test that you can perform is to see whether it is null 
or not. To perform this test, use the is null or is not null operator in a conditional expression. For more 
information, see Is [Not] Null Operator.

If any item (other than a reference variable) in an expression has a null value, the value of the entire expression 
is null, for example:

```
msg = varchar (empno) + ' is not a valid employee number';
```

In this expression, if the variable empno has the value null, msg is null after the statement executes.

If any of the simple variables is null, the result of any comparison involving them is null, for example:

```
count = null;
if count + 1 > 0 then
    callframe newproject;
endif;
```

Because count is null, the result of the comparison that includes count is null. Therefore, the callframe statement 
is never executed.

This rule holds true for more complicated expressions, as in the following statements:

```
man_days = varchar(days) + 'days';
if (start_date + man_days) > 'today' then
    /* processing statements */
endif;
```

If start_date or man_days is null, the entire boolean expression evaluates to null and the processing statements are 
never performed.

# Expressions in SQL Statements

When using expressions in SQL statements in OpenROAD, the operands of the functions and operators (or the entire 
expression) may be literals or database column names. In contexts where correlation names are defined (for example, 
with the select statement), a column name may be preceded by a correlation name (separated from the column name by a 
period).

You can replace any literal with a named constant or simple variable name (or a reference to a simple variable that 
is an attribute of a reference variable or an attribute of a row of an array variable) preceded by a colon. However, 
the field function is not allowed in database statements, nor are procedure or method invocations. SQL function and 
operators are allowed (they are evaluated by the database).

Expressions are composed of various operators and operands that evaluate to a single value or a set of values. Some 
expressions do not use operators; for example, a column name is an expression. Expressions are used in many contexts, 
such as specifying values to be retrieved (in a SELECT clause) or compared (in a WHERE clause). For example:

```sql
SELECT empname, empage FROM employee
WHERE salary > 75000
```

In the preceding example, empname and empage are expressions representing the column values to be retrieved, salary 
is an expression representing a column value to be compared, and 75000 is an integer literal expression.

Expressions that contain aggregate functions can appear only in SELECT and HAVING clauses, and aggregate functions 
cannot be nested.

An expression can be enclosed in parentheses without affecting its value.
