# 3GL Procedures

A 3GL procedure is a procedure written in a third-generation language, such as C, that you can call by name from a script or 4GL procedure. 3GL procedures are used to perform operations that are outside the scope of OpenROAD but are available from a 3GL.

You maintain and compile 3GL procedures outside of OpenROAD. For more information about how to register 3GL procedures in an application, see the Workbench User Guide.

The following subsections describe how to declare parameters for 3GL procedures. They also provide language-specific information about coding the procedures. For more information about calling 3GL procedures, see Using 3GL in Your Application.

## 3GL Parameters

OpenROAD lets you pass simple variables to a 3GL procedure. You can also pass objects and arrays as described in the Language Reference Guide online help and in Using 3GL in Your Application. When you pass values to a 3GL procedure, the data type of the values must match the data types of the parameters receiving them in the procedure.

The following table lists the 4GL simple data types and describes their corresponding 3GL data type declarations:

| 4GL Data Type | Host Variable Declaration |
|---------------|---------------------------|
| Char | Fixed-length, null-terminated character string. The size is determined by field size or declared variable length. Data entered into the field is padded with blank characters up to its full declared length before being passed to the 3GL procedure. |
| Varchar | Variable-length, null-terminated character string. Unlike char type, varchar variables are not extended with blank characters before being passed to the 3GL procedure. |
| date | Fixed-length, 25-byte, null-terminated character string |
| money | Double |
| float, f | Double |
| integer | Long, 4-byte integer |
| smallint | Integer, 4-byte integer |

You can use the byref option with any variable that you pass to a 3GL procedure, regardless of the variable's data type.

You cannot pass a char or varchar variable that contains an embedded zero byte (hexadecimal'00') to a 3GL procedure. No runtime error occurs, but a truncated version of the 4GL variable may be passed.

When a 4GL date variable passes to a 3GL procedure by name, or when the return type of a 3GL procedure is date, 4GL receives it back from the 3GL procedure as a 25-byte string and must then convert it to the internal date format. The date must be valid before the conversion can succeed.

Passing a 4GL variable containing a null value to a 3GL procedure causes a runtime error. Use the ifnull function and is null comparison operator to pass nullable types between 3GL and 4GL, for example:

```
ifnull (v1, v2)
```

This reference returns the value of v1 if v1 is not null; otherwise, if v1 is null, it returns the value of v2. The variables v1 and v2 must be the same data type.

If an impossible value exists for the argument, use the impossible value to indicate a null:

```
callproc empcalc (ifnull (age, -1));
```

If no impossible value exists for the argument, pass a separate indicator variable to indicate a null argument:

```
null_indicator = 0;
if (age is null) then
     null_indicator = -1;
endif;
callproc empcalc (age, null_indicator);
```

## Guidelines for Writing C Procedures

Use the following syntax when writing a C procedure:

```c
procname([parameters])
{
           processing statements
}
```

> Note: You cannot name your C procedure "main," and the procedure must not be static. You can call any C procedure from 4GL except the "main" function.

Follow these guidelines for passing parameters to C procedures:

- Pass an integer as four bytes by value (or by reference if byref is specified).
- Pass a smallint as four bytes by value (or by reference if byref is specified).
- Pass a float as a double-format float by value (or by reference, if byref is specified).

To ensure full portability, pass all floating point parameters to C procedures using the byref qualifier. For example, the following code fragment declares some variables and calls a C procedure, passing the procedure a floating-point parameter:

```c
/* variable declarations */
test_float = float;
test_integer = integer;
test_return = integer
...
test_return = callproc myCproc (test_integer,
                             byref(test_float));
```

The corresponding C procedure is declared as follows:

```c
Long
myCproc (ivalue, fvalue)
long ivalue;
double *fvalue;
{
     processing statements
}
```

- Pass a string as a pointer to a null-terminated string as follows:
  - Pass fixed-length string types (c, char) with trailing blanks up to their full length.
  - Pass variable-length string types (text, varchar) without trailing blanks.

- In this call to a procedure "q":

```c
callproc q (1 + 2, 2.3, 'This is a string');
```

the following declarations are required:

```c
q(x, y, z)
long x;
double y; char *z;
{...
```

To receive x and y that are passed by reference, make the following changes to their formal argument declarations:

```c
long *x;
double *y;
```

No changes are necessary to receive z that is passed by reference.

A C procedure must return an int, a long, a double, or a char * value, as shown in the following examples.

- To return an integer:

```c
int
reti()
{
return 10;
}
```

- To return a floating-point value:

```c
double
retf()
{
return 10.5;
}
```

- To return a character string:

```c
char *
rets()
{
return "Returned from rets";
}
```

Any C procedure that returns a char * value to 4GL must return a pointer to a valid address (a string constant or static or global character buffer). The procedure cannot return a pointer to a local character buffer.
