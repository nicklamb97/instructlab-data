# The `while` Statement in Actian 4GL

The `while` statement in Actian 4GL (Ingres 4GL) is used to create a loop that executes a block of code repeatedly as long as a specified condition is true. This document provides an in-depth explanation of how the `while` statement works, including syntax, usage, and examples.

## Syntax

The basic syntax of the `while` statement in Actian 4GL is as follows:

```4gl
while condition do
    -- statements to execute
endwhile;
```

condition: 
- A logical expression that is evaluated before each iteration of the loop. If the condition evaluates to true, the loop continues; if it evaluates to false, the loop terminates.
statements to execute: 
- The block of code that will be executed repeatedly as long as the condition is true.

## Using endloop for Early Exit
The endloop statement can be used to exit the while loop before the condition becomes false. This is useful for cases where you want to terminate the loop based on some other condition within the loop.

## Basic Loop

This example demonstrates a while loop that prints numbers from 1 to 3.

```4gl
define i integer = 1;

while i <= 3 do
    MESSAGE I;
    let i = i + 1;
endwhile;
```

## Loop with Early Exit Using endloop

This example shows how to use the endloop statement to exit the loop early if a certain condition is met.

```4gl
define i integer = 1;

while true do
    let i = i + 1;
    if i > 3 then
        endloop;
    endif;
endwhile;
```

## Counting Down

This example demonstrates a while loop that counts down from 3 to 1.

```4gl
define i integer = 3;

while i > 0 do
    MESSAGE i;
    let i = i - 1;
endwhile;
```
