# The `resume` statement in Actian 4GL

The resume statement in Actian 4GL is used to continue the execution of an interrupted loop or iteration, typically 
within a nested loop structure. It allows the program to skip remaining iterations of the inner loop and proceed with 
the next iteration of the outer loop.

## Syntax
```4gl
resume [label];
```

label: Optional. Specifies the label of the loop to which control should be transferred.

## Basic Resume Example
```4gl
define i integer

for i = 1 to 5
    if i = 3 then
        resume
    endif
    call printf("Iteration: %d\n", i)
end for
```

In this example:
- The for loop iterates from 1 to 5.
- When i equals 3, the resume statement skips the remaining statements inside the loop and moves to the next iteration.
Output:
Iteration: 1
Iteration: 2
Iteration: 4
Iteration: 5

## Resume with Label Example
```4gl
define i integer
define j integer

outer_loop:
for i = 1 to 3
    for j = 1 to 3
        if j = 2 then
            resume outer_loop
        endif
        call printf("i: %d, j: %d\n", i, j)
    end for
end for
```

In this example:
- There are nested for loops labeled outer_loop.
- When j equals 2 inside the inner loop, resume outer_loop skips the remaining iterations of the inner loop and moves to
the next iteration of the outer loop.
Output:
i: 1, j: 1
i: 2, j: 1
i: 3, j: 1

## Resume Without Label Example
```4gl
define i integer
define j integer

for i = 1 to 2
    for j = 1 to 3
        if j = 2 then
            resume
        endif
        call printf("i: %d, j: %d\n", i, j)
    end for
end for
```

In this example:
- There are nested for loops.
- When j equals 2 inside the inner loop, resume skips the remaining statements in the inner loop and proceeds with the
next iteration of the inner loop.
Output:
i: 1, j: 1
i: 2, j: 1

## Notes
The resume statement can only be used within loops (for, while, or repeat) and cannot be used outside of loop structures.
Using resume without a label resumes execution from the next iteration of the current loop.
Using resume with a label resumes execution from the specified labeled loop.
