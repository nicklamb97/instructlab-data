## Dynamic Names

A dynamic name is a name that you can use in a statement either dynamically (when you run the application) or statically (when you create the application). All dynamic names are underlined in the syntax examples in this guide. Two examples of dynamic names are table names and column names, for example:

```sql
update tablename set columnname = expression
{, columnname = expression} where current of cursor_variable
```

Both statically-specified and dynamically-specified dynamic names must follow these rules:

- The variable that you use as a dynamic name can be a global variable or any variable in the current frame or procedure.

To differentiate between a variable used as a dynamic name and an alphanumeric identifier, put a dereferencing colon (:) before the dynamic name. A dereferencing colon preceding a dynamic name indicates that you are using the contents of a variable to supply the value for an OpenROAD name.

- If the variable is part of an object, put the colon before the full name, for example:

```
callframe :framearray[3].name;
```

Do not place the colon in the middle of the name, for example:

```
callframe framearray[3].:name;
```

Also, in all SQL statements, the colon is required before variable names, for example:

```sql
select from tbl where col1 = :value;
```

Placing the colon before "value" makes the select compare the col1 column with the value of the variable value. If the colon were missing from the statement, the select instead would compare the col1 column with a column named "value."

- Although you can use a nullable variable as a dynamic name, the variable cannot contain a null when it is used in an SQL statement or a 4GL statement because the null causes a runtime error.

The statement that contains the error may be executed, but the value used in place of the variable is zero if a number is expected or "$NULL$ERROR" if a character string is expected, which may cause a runtime error.

- When using dynamic names that are specified statically, do not put quotation marks around the name unless the name is identical to an OpenROAD word.

For more information, see 4GL Keywords.

- When using dynamic names that are specified dynamically, specify a varchar variable.

At runtime, OpenROAD substitutes the current value of the variable for the name.

The advantage of using a dynamically-specified dynamic name is that the values can be substituted at runtime. This behavior allows processing to be guided by user input or other runtime conditions.
