# Subqueries

A subquery is a SELECT statement nested within another SELECT statement.

## Subqueries in the WHERE Clause

A subquery in a WHERE clause can be used to qualify a column against a set of rows.

For example, the following subquery returns the department numbers for departments on the third floor. The outer query retrieves the names of employees who work on the third floor.

```sql
SELECT ename
FROM employee
WHERE dept IN
       (SELECT dno
       FROM dept
       WHERE floor = 3);
```

Subqueries often take the place of expressions in predicates. Subqueries can be used in place of expressions only in the specific instances outlined in the descriptions of Predicates in SQL.

The syntax of the subquery is identical to that of the subselect, except the SELECT clause must contain only one element. A subquery can see correlation names defined (explicitly or implicitly) outside the subquery. For example:

```sql
SELECT ename
FROM employee empx
WHERE salary >
       (SELECT AVG(salary)
       FROM employee empy
       WHERE empy.dept = empx.dept);
```

The preceding subquery uses a correlation name (empx) defined in the outer query. The reference, empx.dept, must be explicitly qualified here. Otherwise the dept column is assumed to be implicitly qualified by empy. The overall query is evaluated by assigning empx each of its values from the employee table and evaluating the subquery for each value of empx.

Note: Although aggregate functions cannot appear directly in a WHERE clause, they can appear in the SELECT clause or the HAVING clause of a subselect, which itself appears in a WHERE clause.

## Subqueries in the FROM Clause (Derived Tables)

Derived tables let you create or simplify complex queries. Useful in data warehousing applications, they provide a way to isolate complex portions of query syntax from the rest of a query.

A derived table is the coding of a SELECT in the FROM clause of a SELECT statement.

For example:

```sql
SELECT relid, x.attname
FROM (SELECT attrelid, attrelidx, attname,
    attfrml FROM iiattribute) x, iirelation
WHERE reltid = attrelid AND reltidx = x.attrelidx
```

The derived table behaves like an inline view; the rows of the result set of the derived table are read and joined to the rest of the query. If possible, the derived table is flattened into the containing query to permit the query compiler to better optimize the query as a whole.

Some complex queries cannot be implemented without using either pre-defined views or derived tables. The derived table approach is more concise than pre-defined views, and avoids having to define persistent objects, such as views, that may be used for a single query only.

For example, consider a query that joins information to some aggregate expressions. Derived tables allow the aggregates to be defined and joined to non-aggregated rows of some other tables all in the same query. Without derived tables, a persistent view would have to be defined to compute the aggregates. Then a query would have to be coded to join the aggregate view to the non-aggregated data.

### Derived Table Syntax

The SELECT in the FROM clause must be enclosed in parentheses and must include a correlation name.

Following the correlation name, the derived table can include an override list of column names in parentheses, or these column names can be coded with AS clauses in the SELECT list of the derived table.

Columns in the derived table can be referenced in SELECT, ON, WHERE, GROUP BY, and HAVING clauses of the containing query, qualified by the correlation name, if necessary.

```sql
SELECT xname FROM (SELECT yname FROM
(SELECT name FROM emp ) y (yname)) x (xname);
```

The "(xname)" and "(yname)" are column name overrides. Not all back ends support this syntax. Instead, use either the name that is generated from the inner select or use regular aliased column syntax.

```sql
SELECT xname FROM (SELECT yname as xname FROM
(SELECT name as yname FROM emp ) y) x;
```

### Example Queries Using Derived Tables

```sql
SELECT e.ename FROM employee e,
    (SELECT AVG(e1.salary), e1.dno FROM employee e1
        GROUP BY e1.dno) e2 (avgsal, dno)
    WHERE e.dno = e2.dno AND e.salary > e2.salary
```

Changing columns names with AS clause:

```sql
SELECT e.ename FROM employee e,
    (SELECT AVG(e1.salary) AS avgsal, e1.dno FROM employee e1
        GROUP BY e1.dno) e2
    WHERE e.dno = e2.dno AND e.salary > e2.salary
```
