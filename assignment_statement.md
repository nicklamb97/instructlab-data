## Assignment Statement

This statement assigns a value produced by an expression to a variable.

This statement has the following syntax:

variable = expression;

### Parameters--Assignment Statement

This statement has the following parameters:

#### variable

Specifies the variable name to which the value is assigned. The variable is always to the left of the assignment operator (=).

#### expression

Specifies any legal 4GL expression. The data types of the expression and the variable must be compatible. The expression is always to the right of the assignment operator (=).

### Simple Assignment Statements

A simple assignment statement assigns the value of an expression to a simple variable, that is, a variable that represents a single data value. A simple variable can be:

- A scalar variable. For example:
  service_fee = 5.50;
- An individual attribute of a reference variable. For example:
  newcustomer.Name = 'Jones';
- An individual data value in an array. For example:
  client[4].Address = '4307 Olive St';

If the variable is associated with a field, OpenROAD displays the new value on the frame when the event block completes.

If you assign a character string to a variable that is shorter than the string, OpenROAD truncates the string. Any value that you assign to any other base data type must fall within the allowable range for the data type or OpenROAD will issue an error.

### Reference Assignment Statements

A reference assignment statement redirects either a reference variable or an array variable. A reference variable is a pointer to an object. An array variable points to a set of reference variables, which points to a set of objects. When you assign a new value to a reference variable or array variable, you point the variable to a new object or set of objects, respectively.

Valid assignments for a reference variable include:

- Another reference variable. For example:
  newcustomer = customer;
- A row reference in a dynamic array. For example:
  newcustomer = client[5];
- A null. For example:
  newcustomer = null;

If two (or more) reference variables point to the same object, you can use either variable to access the individual attributes of the object. Any changes you make are visible through both variables. (If the object's attribute values are displayed in a frame, changes become visible when the event block completes or the form is refreshed.) Similarly, if two array variables point to the same set of objects, you can access the objects or individual object attributes through either variable, and any changes are visible to both variables.

When you assign one reference variable to another, the system class for the reference variable on the right-hand side of the assignment must be the same as, or a subclass of, the variable on the left. In the following example, firstfield is a reference variable of type ActiveField and surname is a reference variable of type EntryField, which is a subclass of ActiveField:

firstfield = surname;

When you assign a null to a reference variable, the variable no longer points to any object.

You can also redirect an array variable. For example, in the following statement both client and old_customers are array variables:

client = old_customers;

The array variable, client, now points to the same array as the old_customers array variable. The declared system class for the array on the right side must be the same as, or a subclass of, the declared system class for the array on the left.

As with reference variables, you can assign a null to an array variable.

Because individual rows of an array are reference variables, you can use individual row references in assignment statements. For example:

client[5] = newcustomer;

### Examples--Assignment Statement

Assign given values to the name and empnum simple variables:

name = 'John';
empnum = 35;

Assign specified values to the columns name and age in the second row of the dynamic array named child:

child[2].name = 'Sally';
child[2].age = 8;

Place the specified values in the name and age columns in the current row of the table field named child:

child[].name = 'Steven';
child[].age = 11;

Assign values to the city attribute of the address and address2 reference variables:

address.City = 'New York';
address2.City = 'Paris';

Reassign the reference variable, address2, to point to the same object as address. Now the city attribute for address2 is the same as that of address:

address2 = address;
currcity = address2.City;
/* Assignment is 'New York' */
