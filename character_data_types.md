# Character Data Types

Character data types are strings of characters. Upper and lower case alphabetic characters are accepted literally. 
There is one fixed-length character data type: char, and two variable-length character data types: varchar and long 
varchar.

The maximum length of a character column cannot exceed 32,000 bytes for a non-UTF-8 installation and 16,000 bytes for 
a UTF-8 installation.

## Char Data Types

Fixed-length char strings can contain any printing or non-printing character, and the null character ('\0'). Char 
strings are padded with blanks to the declared length.

Leading and embedded blanks are significant when comparing char strings. For example, the following char strings are 
considered different:

'A B C' 'ABC'

Length is not significant when comparing char strings; the shorter string is (logically) padded to the length of the 
longer. For example, the following char strings are considered equal:

'ABC' 'ABC '

## Varchar Data Types

Varchar strings are variable-length strings. The varchar data type can contain any character, including non-printing 
characters and the ASCII null character ('\0').

Except when comparing with char data, blanks are significant in the varchar data type. For example, the following two 
varchar strings are not considered equal:

'the store is closed'
and
'thestoreisclosed'

If the strings being compared are unequal in length, the shorter string is padded with trailing blanks until it 
equals the length of the longer string.

For example, consider the following two strings:

'abcd\001'

where:
'\001' represents one ASCII character (ControlA)

and

'abcd'

If they are compared as varchar data types, then

'abcd' > 'abcd\001'

because the blank character added to 'abcd' to make the strings the same length has a higher value than ControlA 
('\040' is greater than '\001').

## Long Varchar Data Types

The intrinsic Ingres DBMS long varchar data type is supported in OpenROAD through the LongVarcharObject system class. 
For more information, see LongVcharObject Class.
