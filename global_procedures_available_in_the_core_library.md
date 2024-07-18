# Global Procedures Available in the Core Library

The following global procedures are available in the core library:

- **_StringParseKeyword**: Returns the values of the specified keyword
- **_StringSub**: Substitutes values for %parameters in a string
- **_StringTokenSub**: Substitutes a value for a single token in a string. Only the first occurrence of the token is replaced.

The following sections describe these procedures.

## The _StringParseKeyword Procedure

The _StringParseKeyword procedure returns the value of the specified keyword in a specially formatted string. This string's format is:

```
"keyword1=value1[;keyword2=value2]"
```

The equal sign separates a keyword from its value. The semicolon must be used to separate the end of a value from the next keyword. There is no space allowed before or after the equal sign. Everything between the semicolon (or the beginning of the string) and the equal sign is treated as part of the keyword. The last value does not have to end with the semicolon. If a value has trailing white space, it will be trimmed. Leading and embedded white space is retained.

The search for the keyword is not case sensitive. Trailing white space on *keyword* is trimmed before the search begins. If the keyword is found, its value is returned. If either the string or the keyword argument is empty, an empty string is returned.

Example syntax for the _StringParseKeyword procedure is:

```
varchar(2000) = _StringParseKeyword(string = varchar(2000), 
           keyword = varchar(32);)
```

The following is an example of how to use the _StringParseKeyword procedure to find the value of a keyword:

```
str1 = 'Name=John Doe;
Address=101 California, USA'; 
value=_StringParseKeyword(string = str1, 
                keyword = 'address');
```

This example returns the string "101 California, USA".

## The _StringSub Procedure

This procedure substitutes values for parameters that are embedded in a specially formatted string. It also substitutes "\t" with HC_TAB, and "\n" with HC_NEWLINE.

The string must be a varchar string embedded with parameters in the format of "%1", "%2", and so forth, up to "%9", and "\t" and "\n". A parameter can occur multiple times in the string.

The syntax for the _StringSub procedure is:

```
varchar(2000) = _StringSub(string=varchar(2000), 
arg1=varchar(100), arg2=varchar(100), arg3=varchar(100), 
arg4=varchar(100), arg5=varchar(100), arg6=varchar(100), 
arg7=varchar(100), arg8=varchar(100), arg9=varchar(100));
```

The following code is an example of this syntax:

```
str1 = _StringSub(string = 'Employee Name:%1, 
    Address:%2(c/o %1)', arg1 = 'John Doe', 
    arg2 ='101 California, USA');
```

This example returns "Employee Name: John Doe, Address:101 California, USA (c/o John Doe)".

## The _StringTokenSub Procedure

This procedure substitutes a value for the specified token in a string. The syntax for this procedure is:

```
varchar(2000) =_StringTokenSub(string=varchar(2000),
              token=varchar(256),
              replacewith=varchar(256)
              [remainingtokens=byref(integer)]);
```

The optional argument remainingtokens, if specified, is set to the number of occurrences of tokens that remain in the string after the substitution.

The following example shows how to use the _StringTokenSub procedure:

```
str1 = _StringTokenSub(string='Employee Name:John Doe, Address: 101 California, USA (c/o John Doe)', 
token = 'John Doe', replacewith='Jane Doe', remaingtoken=byref(icount));
```

This example returns "Employee Name:Jane Doe, Address:101 California, USA (c/o John Doe)". The icount variable contains 1, indicating that there is one more occurrence of John Doe in the substituted string.
