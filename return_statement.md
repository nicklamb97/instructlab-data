# The `return` statement in Actian 4GL

The return statement in Actian 4GL is used to exit a subroutine or function and optionally return a value to the caller. This statement is essential for controlling program flow and passing data back to the calling routine.

## Syntax

```4gl
return [expression];
```

expression: Optional. Specifies the value to be returned to the calling routine.

## Basic Return Example
```4gl
function integer add_numbers(integer a, integer b)
    define result integer
    let result = a + b
    return result
end function

define sum integer
let sum = add_numbers(5, 3)
call printf("Sum: %d\n", sum)
```

In this example:
- The function add_numbers accepts two integers a and b.
- It calculates their sum and returns the result using return result.
- The returned value (8) is assigned to sum, which is then printed.

## Returning Early Example

```4gl
function integer find_index(string array[], string value)
    define i integer
    for i = 1 to dim(array)
        if array[i] = value then
            return i
        endif
    end for
    return 0
end function
```

In this example:
- The function find_index searches for value in array.
- If value is found, it returns the index immediately using return i.
- If value is not found after looping through the array, it returns 0.

## Void Function Example

```4gl
subroutine print_message(string message)
    call printf("%s\n", message)
    return
end subroutine
```

In this example:
- The subroutine print_message prints a message to the console.
- It does not return any value (return without an expression).
- Subroutines in Actian 4GL typically use return without an expression to exit.

## Notes
The return statement can only be used within functions and subroutines.
If an expression is provided, it must match the return type defined in the function or subroutine header.
Using return without an expression exits the function or subroutine immediately.
