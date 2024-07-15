# The `sleep` statement in Actian 4GL

The sleep statement in Actian 4GL is used to pause the execution of a program for a specified amount of time. This can be useful for delaying actions or creating timed pauses in applications.

## Syntax

```4gl
sleep duration
```

duration: Specifies the time to sleep in seconds. The number must be an integer literal or variable.

## Basic Sleep Example

```4gl
define i integer = 1

while i <= 3
do
    call printf("Iteration: %d\n", i)
    let i = i + 1
    sleep 2  -- Sleep for 2 seconds
endwhile
call printf("Finished.\n")
```

## Pause Before User Input
```4gl
define user_input integer

call printf("Please enter a number: ")
call scanf("%d", user_input)

if user_input > 10 then
    call printf("Input is greater than 10.\n")
else
    call printf("Input is less than or equal to 10.\n")
endif

sleep 3  -- Pause for 3 seconds
call printf("End of program.\n")
```

## Notes
The sleep duration cannot be a fractional number. sleep is a blocking operation, meaning it pauses the entire 
program execution during the sleep period.