# Comments

You can include comments anywhere in your 4GL scripts. There are two types of comments:

## End-of-line comments

Single lines or the ends of partial lines can be commented out by preceding the comment with "//". For example, the following is a single-line comment:

```
// This is a single-line comment
```

Here is an end-of-line comment on the second line:

```
insert into employees(salary, age)
values(:salfield, :agefield); //This is a comment
commit work;
```

You may comment out lines of code by preceding them with "//". For example:

```
//insert into employees(salary, age)
//values(:salfield, :agefield);
//commit work;
```

## Block comments

Single-line or multi-line comments can be included in a comment block. A comment block begins with "/*" on the left, ends with "*/". For example, the following is a single-line block comment:

```
/* This is a single-line block comment */
```

The following is a block comment that spans multiple lines:

```
/*
** This is a multi-line block
** comment.
*/
```

Block comments may also be used at the end of a line:

```
insert into employees(salary, age)
values(:salfield, :agefield); /*This is a comment*/
commit work;
```

**Note:** You cannot nest comments.
