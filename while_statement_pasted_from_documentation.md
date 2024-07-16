# While Statement

This statement repeats a series of statements while a specified condition is true.

This statement has the following syntax:

```
[label:] while condition do
               statementlist
         endwhile;
```

The while statement, also called the while loop, executes the series of statements between the do and endwhile keywords as long as the specified condition remains true. The condition expression is tested only at the start of each iteration of the loop. Consequently, if values change during execution of the loop so as to make the condition false, execution still continues through the current iteration of the statement list unless the program encounters an endloop or continue statement.

You can nest while statements in OpenROAD.

## Parameters--While Statement

This statement has the following parameters:

**label**

Specifies a character string identifying each while statement. The label allows an endloop or continue statement to break out of a nested series of while or for statements to a specified level. The label precedes the while keyword and is followed by a colon. It must be a unique alphanumeric identifier and not a keyword.

**condition**

See Condition.

This condition is tested to determine if statementlist is executed.

**statementlist**

See StatementList.

## Breaking Out of While Loops

To break out of a while loop, you can use the endloop, continue, resume, or return statement.

If you use the endloop statement, OpenROAD closes the loop immediately and continues execution with the first statement following the endwhile statement. For example:

```
while name like pattern do
     name = left(name, length(name)-1);
     if name = '' then
          endloop;
     endif;
     /* other statements */
endwhile;
```

If the name is empty, the other statements are not executed in that pass through the loop, and the entire loop is closed.

If you use the continue statement, OpenROAD skips the remaining statements in the statement list and begins the next iteration of the loop. For example:

```
while name like pattern do
     name = left(name, length(name)-1);
     if name like '% ' then
          continue;
     endif;
     /* other statements */
endwhile;
```

If the name has trailing blanks, the other statements are not executed in that pass through the loop, and execution continues with the next iteration of the while loop.

If you use the resume statement, OpenROAD terminates both the loop and the current event block.

If you use the return statement, OpenROAD terminates the current frame, procedure, or method.

You can use nested while statements in 4GL, and you can control breaking out of nested loops by using labels. Both the endloop and the continue statements let you specify labels. Specifying a label with the endloop statement lets you break to a specific level. For example:

```
label1: while name != '' do
     label2: while name like '% ' do
          name = left(name, length(name)-1);
          if name = '' then
               endloop label1;
          elseif length(name) < 3 then
               endloop label2;
          endif;
          /* statementlist3 */
     endwhile;
     /* statementlist4 */
endwhile;
```

There are two possible breaks out of the inner loop. If condition3 is true, both loops are closed, and control resumes at the statement following the outer loop. If condition4 is true, only the inner loop is closed, and execution continues at the beginning of statementlist4.

If no label is specified after endloop, OpenROAD terminates only the loop that issued the endloop statement.

Specifying a label with the continue statement lets you specify with which loop you want to continue. For example:

```
loop1: while condition1 do
     /* statementlist1 */
     loop2: while condition2 do
          /* statementlist2 */
          if condition3 then
               continue loop1;
          endif;
          /* statementlist3 */
     endwhile;
     /* statementlist4 */
endwhile;
```

Whenever condition3 occurs, loop2 is abandoned and control passes to loop1.

## Examples--While Statement

Repeat a prompt five times or until a valid answer is given:

```
answer = prompt 'Please answer Y or N';
requests = 0;
while(lowercase(left(answer,1)) != 'y' and
     lowercase(left(answer,1)) != 'n') and
     requests < 5 do
          answer = prompt 'Please answer Y or N:';
          requests = requests + 1;
endwhile;
```

Use the while statement to handle deadlock:

```
tries = 1;
while tries <= 10 do
          /* insert statement to try to insert
          ** into database
          */
     if iierrornumber = 0 then
          /* Success */
          commit work;
          message 'Insert successful';
          tries = 10;
     else
          message 'Insert failed due to db error';
          rollback work;
          /* Try again on deadlock */
          /* OpenSQL Serialization Failure */
          if iierrornumber = -49900 then
               tries = tries + 1;
          else
               endloop;
          endif;
     endif;
endwhile;
```
