# Nulls

A null represents an undefined or unknown value and is specified by the keyword null. A null is not the same as a 
zero, a blank, or an empty string. A null can be assigned to any nullable variable when no other value is 
specifically assigned.

You can assign a null to any nullable variable by using the null constant. For example, assume that your program 
contains a nullable variable named age. To set this variable to null, use the following statement:

```
age = null;
```

However, you cannot use the following statement:

```
if x = null ...
```

Instead, you must use the following statement:

```
if x is null ...
```

If you set the value of a reference variable to null, the reference variable is no longer associated with an object. 
(OpenROAD frees any object that does not have at least one reference variable pointing at it.)

The ifnull() function and the is [not] null operator provide you with ways to handle nulls in expressions. For more 
information about the is null operator, see Is [Not] Null Operator.
