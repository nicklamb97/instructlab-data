# Dbmsinfo Function

In OpenROAD, if you use the dbmsinfo function as a statement by itself, the only valid request_name is **_bintim**:

```
variable = dbmsinfo('_bintim')
```

All other values of request_name return blanks unless the function is part of a larger SQL statement. For example, the following statement returns a user name:

```sql
select logname = dbmsinfo('username')
```

However, the following statement returns blanks:

```
logname = dbmsinfo('username')
```
