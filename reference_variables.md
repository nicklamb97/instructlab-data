# Reference Variables

A reference variable is a variable that points to an object of a given class, letting you access the value of an 
object. An object is a compound data structure that holds values that you can manipulate. A reference variable does 
not store its own values. Instead, when you reference the reference variable, OpenROAD uses the values in the 
corresponding object. The object is made up of attributes, which can be simple variables, reference variables, or 
array variables.

The class of an object defines its attributes. OpenROAD includes both system classes and user-defined classes.

In your 4GL code, when you reference a reference variable, you can work with the entire object as a single unit. For 
example, you can retrieve a row from a database table and assign all values from the row to a single object and then 
pass the object to a called procedure. You can also work with the individual attributes in the object.

## How You Can Declare Reference Variables

To declare a reference variable, use the following syntax:

```
name = class [ [with] default null]
```

The value of class can be any named user class or system class (for a complete list of system classes, see the 
Language Reference Guide online help). The following example declares a reference variable of the StringObject class:

```
strobj = StringObject;
```

All reference variables are nullable.

When you create a field on a form, you specify the variable's name and data type on the field's property inspector in 
the OpenROAD Workbench. Unless you specify otherwise, OpenROAD automatically declares the variable that you use to 
access the data in that field. If the field is a named composite field, such as a stackfield or subform, the variable 
is a reference variable and you may or may not define its data type (class).

If you do define the data type for the reference variable, the data type must be a system or user class name. You can 
choose to exclude fields for some of the attributes (this provides a way to keep additional undisplayed control 
information).

If you do not name the class when you create a composite field, OpenROAD creates an unnamed class that you can 
reference only within the frame that contains the composite field.

## How You Can Use Reference Variables

In OpenROAD, you can work with an object as a whole or with the attributes of the object. To manipulate the object as 
a single unit, use the name of the object's reference variable in your 4GL code.

For example, you can create a subform and specify its variable's class as a named user class. In your program you 
could then pass the values in the subform fields to a procedure simply by naming the variable as a procedure 
parameter.

In the following statement, customer is the name of the variable associated with a subform:

```
callproc update_cust (new = customer)
```

To access an individual attribute that belongs to an object, use the dot notation:

```
referencevariable_name.attribute_name
```

For example, assume that the subform previously described contained a field called Address. To reference that field, 
you would use:

```
customer.Address
```

In the following example, City and Street are attributes of class addr, and address is a declared reference variable:

```
address.City = 'Dallas';
x = address.Street;
callframe delete_old (address = address.City);
```

OpenROAD checks references to attributes by name and produces compiler errors if the variable's class does not 
contain those attributes. For example, given the following declarations:

```
a = Addr;
b = Addr;
tmp_char = varchar(30) not null;
```

Some valid references would be:

```
a.Street = tmp_char;
tmp_char = a.city;
```

Some invalid references would be:

```
a.Province = tmp_char;  /*  ERROR at compile time
                        ** because attribute 'province'
                        ** is not defined for class 'addr' */
a = null;
a.Street = tmp_char;    /*  ERROR at runtime because 'a' is
                        ** null, and does not reference
                        ** an object */
```

## Null Reference Variables

When you declare a reference variable without specifying default null, the variable points to an object whose 
attributes are automatically initialized with the default values given in the definition of the object. However, you 
can set a reference variable to null. A reference variable that is set to null does not point to any object. Whenever 
all reference variables for an object have been redirected or set to null, OpenROAD frees the associated object, for 
example, given the following declarations:

```
a = Addr;
b = Addr;
```

The consequences of resetting the variables a and b would be:

```
a = b;     /*  The original object for a is freed. Now
            ** both a and b point to the same object. */
a = null;  /*  The object created for b still has 1
            ** reference (b). */
b = null:  /*  The object created for b is freed. */
```

As an alternative, you can specify default null on the declaration and a default object is not created.

You can check for a null reference variable with the is null operator, for example:

```
if a is null then
    /* processing statements */
endif;
```

For more information about null reference variables, see Nulls in Expressions.

## How You Can Create Objects for Reference Variables

You may need to create an object for a reference variable. For example, dynamic programs can create fields or forms 
at runtime by using the Create method, defined for the Class class. The Create method returns an object of a 
specified class.

In the following example, the reference variable a is declared with a default value of null:

```
a = Addr default null;
```

And a new object is created for it:

```
a = Addr.Create();    /* Creates an Addr object */
a.Street = 'main street';
a.City = 'dallas';
```

For more information about the Create method, see the Class class Create Method. Examples of using the Create method 
are provided in "Creating a Frame at Runtime" in the Programming Guide.
