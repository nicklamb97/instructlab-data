# Simple Variables

A simple variable is a single data item. It contains only one value. A simple variable can be any of the basic data 
types, such as integer or varchar, with the exception of table_key and object_key as described in Data Types.

## How You Can Declare Simple Variables

The syntax for declaring a simple variable is:

*name* = *datatype* [with null|not null] [not default|with default|[with] default *defaultvalue*]

The following example shows a local variable declaration for an integer variable:

```
i = integer;
```

By default, all simple variables are nullable. If you do not want the variable to be nullable, you must include the 
not null clause in the declaration, for example:

```
i = integer not null;
```

By default, all numeric simple variables are assigned a default value of zero, and all character simple variables are 
assigned a default value of the empty string (''). If you want to specify a different default value, you must include 
the default clause. The default value must be null, a literal, or one of the system constants defined in the 
*Language Reference Guide* online help, for example:

```
i = integer not null default false;
```

The with null, not default, and with default (without a value) clauses are provided for syntactic compatibility with 
SQL, but they have no effect in OpenROAD.

## How You Can Use Simple Variables

To make use of a simple variable, use the variable name. For example, to assign the value of Smith to the person 
variable, use the following statement:

```
person = 'Smith';
```

Individual elements in a reference variable or an array can be simple variables, as shown in the illustration in 
Variables. In this illustration, Film is a reference variable with three attributes. Each of these attributes is a 
simple variable. Similarly, Movies is an array and each individual element in the array is a simple variable.

In a 4GL script, you can use simple variables as reference or array variable elements in any context that you can use 
other simple variables, for example:

```
film.Director = 'Hitchcock';
if movies[].Title = 'ANNIE HALL' then ...
```

In the first example, film.Director is a simple variable that is an element of a reference variable. In the second, 
movies[].Title is a simple variable that is an element of an array.

For information about referencing the individual elements in a reference or array variable, see Reference Variables 
and Dynamic Array Variables.
