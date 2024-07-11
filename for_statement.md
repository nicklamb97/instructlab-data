## For Statement

This statement repeats a series of statements, incrementing or decrementing an index variable.

This statement has the following syntax:

```
[label:] for indexvariable = startexpression
                    to|downto endexpression
                    do statementlist
        endfor;
```

The for statement, also called the for loop, repetitively executes the series of statements in the statementlist between the do and endfor keywords. The specified indexvariable is incremented (if to is specified) or decremented (if downto is specified) after each iteration of the series of statements. The series of statements is executed until the indexvariable exceeds (to) or becomes less than (downto) the specified endexpression. This test is performed before each iteration (thus, it is possible for no iterations to be executed). The specified endexpression is re-evaluated for each test.

**Note:** You can nest for statements in OpenROAD.

### Breaking Out of For Loops

To break out of a for loop, you can use the endloop, continue, resume, or return statement.

If you use the endloop statement, OpenROAD closes the loop immediately and continues execution with the first statement following the endfor statement. For example:

```
for i = 1 to arr.Lastrow do
     /* statementlist1 */
     if arr.Name = '' then
          endloop;
     endif;
     /* statementlist2 */
endfor;
```

If condition is true, statementlist2 is not executed in that pass through the loop, and the entire loop is closed.

If you use the continue statement, OpenROAD skips the remaining statements in the statement list, increments or decrements the index variable, and begins the next iteration of the loop. For example:

```
for i = 1 to arr.Lastrow do
     /* statementlist1 */
     if arr.Name = '' then
          continue;
     endif;
     /* statementlist2 */
endfor;
```
If condition is true, statementlist2 is not executed in that pass through the loop, and execution continues with the next iteration of the loop.

If you use the resume statement, OpenROAD terminates both the loop and the current event block.

If you use the return statement, OpenROAD terminates the current frame, procedure, or method.

You can use nested for statements in 4GL, and you can control breaking out of nested loops by using labels. Both the endloop and the continue statements let you specify labels. Specifying a label with the endloop statement lets you break to a specific level. For example:

```
label1: for i = 1 to arr1.Lastrow do
     /* statementlist1 */
     label2: for j = 1 to arr2.Lastrow do
          /* statementlist2 */
          if arr.Name = '' then
               endloop label1;
          elseif arr.FullTime = FALSE then
               endloop label2;
          endif;
          /* statementlist3 */
     endfor;
     /* statementlist4 */
endfor;
```
There are two possible breaks out of the inner loop. If condition1 is true, both loops are closed, and control resumes at the statement following the outer loop. If condition2 is true, only the inner loop is closed, and execution continues at the beginning of statementlist4.

If no label is specified after endloop, OpenROAD terminates only the loop that issued the endloop statement.

Specifying a label with the continue statement lets you specify which loop you want to continue. For example:
```
loop1: for i = 1 to arr1.Lastrow do
     /* statementlist1 */
     loop2: for j = 1 to arr2.Lastrow do
          /* statementlist2 */
          if arr.FullTime = FALSE then
               continue loop1;
          endif;
          /* statementlist3 */
     endfor;
     /* statementlist4 */
endfor;
```
Whenever condition occurs, loop2 is abandoned and control goes to the next iteration of loop1.

### Parameters--For Statement

This statement has the following parameters:

#### label

Specifies a character string identifying each for statement. The label allows an endloop or continue statement to break out of a nested series of for or while statements to a specified level. The label precedes the for keyword and is followed by a colon. The label must be a unique alphanumeric identifier and not a keyword.

#### indexvariable

Specifies an integer variable that is incremented (to) or decremented (downto) after each iteration of the for loop. Its contents upon exit from the for loop are undefined.

#### startexpression

Specifies an integer expression to which the indexvariable is set before the first iteration of the for loop

#### endexpression

Specifies an integer expression to which indexvariable is compared before each iteration of the for loop (including the first iteration). It is re-evaluated for each comparison.

If indexvariable > endexpression (to) or indexvariable < endexpression (downto), the for loop is terminated.

#### statementlist

See StatementList.

### Example--For Statement

Use the for loop to increment through an array:

```
for i = 1 to array.Lastrow do
     arr[i].PaymentReceived = TRUE;
endfor;
```
