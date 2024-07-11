## Endloop Statement

The endloop statement is used to terminate a for, select, or while loop. 

It has the following syntax:
```
endloop [label];
```

The endloop statement terminates a for, select, or while loop. The statement then transfers control to the first statement following the terminated loop's endfor, end, or endwhile statement.
The endloop statement is unlike the resume statement, which can be used to terminate both the loop and the current event block. It differs also from the continue statement, which skips over statements in a loop but continues with the next iteration.
For more information about loop statements, see the descriptions of the continue statement (see Continue Statement), the for statement (see For Statement), the select statement (see Select Statement), and the while statement (see While Statement).

### Parameters--Endloop Statement

This statement has the following parameter:

#### label
  Specifies which loop you want to terminate. This parameter must be a character string without quotation marks and it cannot be a keyword.

### Examples--Endloop Statement
Break out of the loop labeled “B” if condition3 is true. This statement terminates both the inner while loop containing the endloop statement and the outer while loop labeled “B” in which it is nested. If condition3 is false, the continue statement goes to the next iteration of the inner loop:

```
B:        while condition1 do
          /* statements */
          while condition2 do
               /* statements */
               if condition3 then
                    endloop B;
               else
                    continue;
               endif;
          endwhile
endwhile;
```
