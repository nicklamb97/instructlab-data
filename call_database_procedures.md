# How You Can Call Database Procedures

A *database procedure* is a data-oriented procedure stored in the database and executed within the database server that you can call by name in a script or 4GL procedure. Database procedures are often used to increase performance and help ensure data integrity and consistency.

In OpenROAD, you call database procedures the same way you call 4GL procedures.

## Syntax

The syntax for calling a database procedure is:

```sql
[integer_variable = ] callproc procedurename
               [(parameter = expression
               {, parameter = expression})]
```

### *procedurename*

Specifies the name you specified when you created the database procedure and that you later provided when you registered the database procedure in OpenROAD Workbench.

In the following example, the callproc statement calls the database procedure named mydbprocedure:

```sql
callproc mydbprocedure;
```

## How You Can Pass Parameters to and From Database Procedures

You use parameter names to pass parameters to database procedures just as you do with 4GL procedures. You can pass the parameters in any order. For database procedures, the parameters must be simple variables only.

In the following example, intparam, floatparam, and charparam are integer, float, and varchar variables, respectively:

```sql
returnvalue = callproc mydbprocedure(intparam = 256,
    floatparam = 3.625,
    charparam = 'seen any good movies lately');
```

When the procedure returns a value, you must specify an integer variable in the calling frame or procedure to receive the return value, for example:

```sql
returnvalue = integer;
```
