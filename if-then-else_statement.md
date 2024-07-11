## If-Then-Else Statement

This statement chooses between alternative paths of execution. You can use the if-then-else statement to control the flow of an application by establishing a variety of logical relationships.

This statement has the following syntax:
```
if condition then statementlist
          [{elseif condition then statementlist}]
          [else statementlist]
endif;
```
You can nest if statements.

### Parameters--If-Then-Else Statement

This statement has the following parameters:

#### condition

See Condition. This condition is tested to determine which statementlist is executed.

#### statementlist

See StatementList.

### Variants--If-Then-Else Statement

The if-then-else statement has the following variants:

#### if-then-endif

The if statement tests for the truth of an expression. If the expression is true, OpenROAD executes the set of statements following the then keyword up to the endif statement. If the expression is false or null, OpenROAD executes the statements after the endif statement.

#### if-then-else-endif

If the expression in the if statement is true, 4GL executes the set of statements following the then keyword up to the else keyword. If the expression is false or null, OpenROAD executes the set of statements following the else keyword up to the endif keyword.

#### if-then-elseif-then-else-endif

If the expression in the if statement is true, OpenROAD executes the set of statements following the then keyword up to the elseif keyword. If the expression is false or null, then OpenROAD tests the expression that starts with the elseif keyword. If this expression is true, OpenROAD executes the set of statements up to the else keyword.

If both conditions are false or null, then OpenROAD executes the set of statements following the else keyword up to the endif keyword.

### Examples--If-Then-Else Statement

Set a status variable if no employee number was entered in the empnum field:
```
if empnum = 0 then
     employee_number = 'None';
endif;
```
Prompt for a value if no employee number was entered in the empnum field:
```
if empnum = 0 then
     enum = prompt 'Please enter employee number';
endif;
```
Control the flow of frames within an application based on the value in the status variable in the current frame:
```
if status = 'n' then
     callframe new;
elseif status = 'c' then
     callframe completed;
else
     callframe inprogress;
endif;
```
Nest multiple if statements:
```
if status = 'n' then
     if empnum = 0 then
          employee_number = 'None';
     else
          callframe NewEmp;
     endif;
endif;

```
