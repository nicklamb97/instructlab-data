# Get Global/Set Global Statement
This statement accesses global variables and constants.
This statement has the following syntax:

```4gl
exec 4gl get global variable (:var[:ind] = name);
exec 4gl set global variable (name = :var[:ind]);
exec 4gl get global constant (:var[:ind] = name);
```

These statements enable 3GL code to access 4GL global variables and constants, including structured data, for which var is a handle:
var:
- Specifies a 3GL variable
ind:
- Specifies a 3GL null indicator variable
name:
- Specifies the name of the global variable or global constant being retrieved