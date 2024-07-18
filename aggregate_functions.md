# Aggregate Functions

Aggregate functions take a set of values as their argument.

Aggregate functions include the following:
• Unary
• Binary
• Count

## Unary Aggregate Functions

A unary aggregate function returns a single value based on the contents of a column. Aggregate functions are also called set functions.

Note: Aggregate functions used in OpenROAD can only be coded inside SQL statements.

The following example uses the sum aggregate function to calculate the total of salaries for employees in department 23:

```sql
SELECT SUM (employee.salary)
       FROM employee
       WHERE employee.dept = 23;
```

## SQL Aggregate Functions

The following table lists SQL aggregate functions:

Aggregate Function Name
Result Data Type
Description
ANY
integer
Returns 1 if any row in the table fulfills the where clause, or 0 if no rows fulfill the where clause.
AVG
float, money, date (interval only)
Average (sum/count)
The sum of the values must be within the range of the result data type.
COUNT
integer
Count of non-null occurrences
MAX
same as argument
Maximum value
MIN
same as argument
Minimum value
SUM
integer, float, money, date (interval only)
Column total
STDDEV_POP
float
Compute the population form of the standard deviation (square root of the population variance of the group).
STDDEV_SAMP
float
Computes the sample form of the standard deviation (square root of the sample variance of the group).
VAR_POP
float
Computes the population form of the variance (sum of the squares of the difference of each argument value in the group from the mean of the values, divided by the count of the values).
VAR_SAMP
float
Computes the sample form of the variance (sum of the squares of the difference of each argument value in the group from the mean of the values, divided by the count of the values minus 1).

The general syntax of an aggregate function is as follows:

```
function_name ([DISTINCT | ALL] expr)
```

where function_name denotes an aggregate function and expr denotes any expression that does not include an aggregate function reference (at any level of nesting).

To eliminate duplicate values, specify DISTINCT. To retain duplicate values, specify ALL, which is the default. DISTINCT is not meaningful with the functions MIN and MAX because these functions return single values (and not a set of values).

Nulls are ignored by the aggregate functions, with the exception of COUNT, as described in COUNT(*) Function.

## Binary Aggregate Functions

Ingres supports a variety of binary aggregate functions that perform a variety of regression and correlation analysis. For all of the binary aggregate functions, the first argument is the independent variable and the second argument is the dependent variable.

The following table lists binary aggregate functions:

Binary Aggregate Function Name
Result Data Type
Description
REGR_COUNT
(indep_parm, dep_parm)
integer
Count of rows with non-null values for both dependent and independent variables.
COVAR_POP
(indep_parm, dep_parm)
float
Population covariance (sum of the products of the difference of the independent variable from its mean, times the difference of the dependent variable from its mean, divided by the number of rows).
COVAR_SAMP
(indep_parm, dep_parm)
float
Sample covariance (sum of the products of the difference of the independent variable from its mean, times the difference of the dependent variable from its mean, divided by the number of rows minus 1).
CORR
(indep_parm, dep_parm)
float
Correlation coefficient (ratio of the population covariance divided by the product of the population standard deviation of the independent variable and the population standard deviation of the dependent variable).
REGR_R2
(indep_parm, dep_parm)
float
Square of the correlation coefficient.
REGR_SLOPE
(indep_parm, dep_parm)
float
Slope of the least-squares-fit linear equation determined by the (independent variable, dependent variable) pairs.
REGR_INTERCEPT
(indep_parm, dep_parm)
float
Y-intercept of the least-squares-fit linear equation determined by the (independent variable, dependent variable) pairs.
REGR_SXX
(indep_parm, dep_parm)
float
Sum of the squares of the independent variable.
REGR_SYY
(indep_parm, dep_parm)
float
Sum of the squares of the dependent variable.
REGR_SXY
(indep_parm, dep_parm)
float
Sum of the product of the independent variable and the dependent variable.
REGR_AVGX
(indep_parm, dep_parm)
float
Average of the independent variables.
REGR_AVGY
(indep_parm, dep_parm)
float
Average of the dependent variables.

## COUNT(*) Function

The COUNT function can take the wildcard character (*) as an argument. This character is used to count the number of rows in a result table, including rows that contain nulls.

For example, the following statement counts the number of employees in department 23:

```sql
SELECT COUNT(*)
       FROM employee
       WHERE dept = 23;
```

The asterisk (*) argument cannot be qualified with ALL or DISTINCT.

Because COUNT(*) counts rows rather than columns, it does not ignore nulls. Consider the following table:

Name
Exemptions
Smith
0
Jones
2
Tanghetti
4
Fong
Null
Stevens
Null

Running:

```sql
COUNT(exemptions)
```

returns the value of 3, whereas:

```sql
COUNT(*)
```

returns 5.

Except COUNT, if the argument to an aggregate function evaluates to an empty set, the function returns a null. The COUNT function returns a zero.

## Aggregate Functions and Decimal Data

Given decimal arguments, aggregate functions (with the exception of COUNT) return decimal results.

The following table explains how to determine the scale and precision of results returned for aggregates with decimal arguments:

Function Name
Precision of Result
Scale of Result
COUNT
Not applicable
Not applicable
SUM
39
Same as argument
AVG
39
Scale of argument + 1 (to a maximum of 39)
MAX
Same as argument
Same as argument
MIN
Same as argument
Same as argument

## GROUP BY Clause with Aggregate Functions

The GROUP BY clause allows aggregate functions to be performed on subsets of the rows in the table. The subsets are defined by the GROUP BY clause.

For example, the following statement selects rows from a table of political candidates, groups the rows by party, and returns the name of each party and the average funding for the candidates in that party.

```sql
SELECT party, AVG(funding)
       FROM candidates
       GROUP BY party;
```

## Restrictions on the Use of Aggregate Functions

The following restrictions apply to the use of aggregate functions:

• Aggregate functions cannot be nested.
• Aggregate functions can be used only in SELECT or HAVING clauses.
• If a SELECT or HAVING clause contains an aggregate function, columns not specified in the aggregate must be specified in the GROUP BY clause. For example:

```sql
SELECT dept, AVG(emp_age)
FROM employee
GROUP BY dept;
```

The above SELECT statement specifies two columns, dept and emp_age, but only emp_age is referenced by the aggregate function, AVG. The dept column is specified in the GROUP BY clause.
