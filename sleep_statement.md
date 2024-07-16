# The `sleep` statement in Actian 4GL

The sleep statement in Actian 4GL is used to pause the execution of a program for a specified amount of time. This can be useful for delaying actions or creating timed pauses in applications.

## Syntax

```4gl
sleep duration;
```

duration: Specifies the time to sleep in seconds. The number must be an integer literal or variable.

## Basic Sleep Example

```4gl
i = integer;
i = 1;

WHILE i <= 3 DO
    MESSAGE i;
    let i = i + 1;
    sleep 2;
ENDWHILE;
```

## Pause Before User Input
```4gl
user_input = 10;
IF user_input > 10 then
    MESSAGE "Input is greater than 10.";
ELSE
    MESSAGE "Input is less than or equal to 10.";
ENDIF;

sleep 3;
MESSAGE "End of program.";
```

## Notes
The sleep duration cannot be a fractional number. sleep is a blocking operation, meaning it pauses the entire 
program execution during the sleep period.