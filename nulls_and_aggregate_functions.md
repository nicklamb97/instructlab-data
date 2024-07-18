# Nulls and Aggregate Functions

If an aggregate function against a column that contains nulls is executed, the function ignores the nulls. This prevents unknown or inapplicable values from affecting the result of the aggregate. For example, if the aggregate function, `avg()`, is applied to a column that holds the ages of your employees, be sure that any ages that have not been entered in the table are not treated as zeros by the function. This distorts the true average age. If a null is assigned to any missing ages, the aggregate returns a correct result: the average of all known employee ages.

Aggregate functions, except `count()`, return null for an aggregate that has an argument that evaluates to an empty set. (`Count()` returns 0 for an empty set.) In the following example, the select returns null, because there are no rows in the table named test.

```sql
create table test (col1 integer not null);
select max(col1) as x from test;
```

In the above example, use the `ifnull` function to return a zero (0) instead of a null:

```sql
select ifnull(max(coll),0) as x from test;
```

When specifying a column that contains nulls as a grouping column (that is, in the `group by` clause) for an aggregate function, nulls in the column are treated as equal for the purposes of grouping. This is the one exception to the rule that nulls are not equal to other nulls.
