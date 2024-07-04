# Dynamic Array Variables

A dynamic array variable is a named set of rows. All the rows in the array are reference variables that point to objects of a given class (or any of its subclasses).

Rows in an array are numbered. Because you can add to or subtract rows from an array and the array automatically adjusts as you do so, the array is said to be dynamic.

Some arrays have rows with non-positive numbers. These are rows that are marked "deleted." Deleted rows are not visible when the array is displayed in a table field, but they are not yet actually removed from the array.

Each array is associated with an array object, that is, an object of the class ArrayObject. The methods that you can use to manipulate arrays are defined for this class as described in ArrayObject Class.

The following subsections describe how to declare a dynamic array and briefly discuss referencing arrays in your 4GL code. For more information about referencing array components, manipulating arrays, and the relationship between arrays and table fields, see "Working with Arrays, Table Fields, and Collections" in the Programming Guide.

## How You Can Declare Dynamic Array Variables

To declare an array variable, specify any system class or user class for the objects associated with the variable. The syntax is:

```
name = array of class [with] default null]
```

The following code is an example:

```
emparray = array of Emp;
```

When you declare an array, it is empty. OpenROAD does not populate the array automatically. There are a variety of methods that you can use to add rows to an array. For more information, see "Working with Arrays, Table Fields, and Collections" in the Programming Guide.

## How You Can Reference Dynamic Array Variables

You can reference an array as a whole unit or you can reference the individual elements of the array—a single row, a single column, or a single cell.

By referencing the array itself, you can work with the entire set of objects as a unit. For example, you can pass the entire array as a parameter to a procedure or you can retrieve a group of rows from a database table into a single array. You can also get information about the array and manipulate the array.

To reference an array as a unit, use the array name. For example, assume that movies is the name of an array and that you want to pass this array to the movie_list frame. You could use the following statement:

```
callframe movie_list (mv_fld = movies);
```

By referencing an individual row, you can work with an individual object in the array. To reference an individual row, use the following syntax:

```
arrayname[n]
```

where arrayname is the name of the array in which the row resides and n is the number of the row. For example, to pass one of the rows from the movies array to a procedure:

```
callproc update_row (mv_row = movies[i]);
```

By referencing a column, you can work with one attribute across all the objects. The syntax is:

```
arrayname[*].columnname
```

where arrayname is the name of the array and columnname is the name of the column. References to array columns are most commonly used to detect events, such as Entry or Exit events, on the column when the array is displayed in a table field, for example:

```
on entry movies[*].title...
```

To reference an individual element of an array, use the following syntax:

```
arrayname[n].columnname
```

where arrayname is the name of the array, n is the number of the row, and columnname is the name of the column.

An individual element of an array can be a reference variable or an array. This means that arrays can contain nested objects or arrays. You can also reference the individual elements of these nested objects or arrays in your 4GL code. For a nested object, the syntax is:

```
arrayname[n].nestedobject.attribute
```

where arrayname is the name of the object, n is the number of the row, nestedobject is the name of the nested object, and attribute is any attribute of that nested object.

For a nested array, the syntax is:

```
arrayname[n].nestedarrayname[x].attribute
```

where arrayname is the name of the outer array, n is the row number in that array, nestedarrayname is the name of the nested array, x is the number of the row within the nested array, and attribute is any attribute of the nested array.
