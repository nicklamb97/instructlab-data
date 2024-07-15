# The `prompt` statement in Actian 4GL

The `prompt` statement in Actian 4GL is used to display a message to the user and wait for input from the user. It is 
commonly used to interactively gather input from the user during program execution.

## Syntax

```4gl
prompt "message" variable [help "help_message"] [options] [at position row, column] [with attributes attribute]
```

- message: Required. Specifies the message displayed to the user.
- variable: Required. Specifies the variable to store the user's input.
- help: Optional. Specifies a help message displayed to the user.
- options: Optional. Specifies options available to the user.
- position: Optional. Specifies the position on the screen where the prompt is displayed.
- attributes: Optional. Specifies display attributes such as color, highlighting, etc.

## Basic Prompt Example
```4gl
define name char(50)

prompt "Enter your name: " name
call printf("Hello, %s!\n", name)
```

In this example:
- The prompt statement displays "Enter your name: " to the user.
- The user enters their name, which is stored in the variable name.
- The program then prints a greeting using the entered name.

## Prompt with Help Example
```4gl
define age integer

prompt "Enter your age: " age help "Please enter your age in years."
call printf("You are %d years old.\n", age)
```

In this example:
- The prompt statement displays "Enter your age: " to the user.
- It also provides a help message "Please enter your age in years." to assist the user.
- The user enters their age, which is stored in the variable age.
- The program then prints the entered age.

## Prompt with Options Example
```4gl
define choice char(1)

prompt "Choose an option (A, B, C): " choice options "A, B, C"
call printf("You chose option %s.\n", choice)
```

In this example:
- The prompt statement displays "Choose an option (A, B, C): " to the user.
- It specifies options "A, B, C" that the user can choose from.
- The user selects an option, which is stored in the variable choice.
- The program then prints the chosen option.

## Notes
The prompt statement is primarily used for interactive input from the user.
It supports displaying a message, providing help, specifying options, and setting display attributes.
The entered value is stored in the specified variable for further processing within the program.