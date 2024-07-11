## Continue Statement

This statement skips the remainder of a loop iteration and then continues processing the next iteration of the loop.

This statement has the following syntax:

```
continue [label];
```
You can use the continue statement in for, select, and while loops. For more information about other loop statements, see:

- Endloop Statement
- For Statement
- Select Statement
- While Statement

### Parameters--Continue Statement

This statement has the following parameter:

#### label

Specifies the loop that you want to continue using. This must be a character string without quotation marks and cannot be a keyword (see 4GL Keywords).

If you do not specify a value for a label, OpenROAD continues with the innermost loop.

### Examples--Continue Statement

Process all changed rows:

```
i = 1;
while i <= x.LastRow do
     if x[i]._RowState != RS_CHANGED then
          i = i + 1;
          continue;
     endif;
     /* Process row */
     i = i + 1;
endwhile;
```

In the following example, the loop1 and loop2 labels are used to specify the outer and inner loops in this code sequence. The first continue statement skips the inner loop when condition3 is true. The loop2 label could be omitted, but is used for program clarity:

```
loop1:     while condition1 do
     /* statements */
     loop2:     while condition2 do
          /* statements */
          if condition3 then
               continue loop1;
          else
               continue loop2;
          endif;
   endwhile;
endwhile;
```
