
# Nulls and Integrity Constraints

When creating a table with nullable columns and subsequently creating integrities on those columns (using the CREATE INTEGRITY statement), the constraint must include the `OR...IS NULL` clause to ensure that nulls are allowed in that column.

For example, if the following create table statement is issued:

```sql
create table test (a int, b int not null); /* "a" is nullable */
```

and the following integrity constraint is defined on the test table:

```sql
create integrity on test is a > 10;
```

the comparison, `a > 10`, is not true whenever `a` is null. For this reason, the table does not allow nulls in column `a`, even though the column is defined as a nullable column.

Similarly, the following INSERT statements fails:

```sql
insert into test (b) values (5);
insert into test values (null, 5);
```

Both of these INSERT statements are acceptable if the integrity had not been defined on column `a`. To allow nulls in column `a`, define the integrity as:

```sql
create integrity on test is a > 10 or a is null;
```

**Note:** If an integrity on a nullable column is created without specifying the `OR...IS NULL` clause and the column contains nulls, the DBMS Server issues an error and the integrity is not created.
