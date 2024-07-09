# Understanding FOR Loops in Actian 4GL

The `FOR` loop is an essential control structure in Actian 4GL, allowing repetitive execution of a block of code for a specified number of iterations or through a sequence of values. This document will provide a detailed explanation of how `FOR` loops function in Actian 4GL, including syntax, usage, and examples.

## Basic Syntax

The `FOR` loop in Actian 4GL can be used in different ways, such as iterating over a range of values or through a set of records. Below is the basic syntax for each type.

### Iterating Over a Range

```4gl
FOR variable = start_value TO end_value [STEP step_value]
    statements
END FOR
```

### Iterating Through a Set of Records

```4gl
FOR EACH record IN table
    statements
END FOR
```

## Key Points

- variable: A control variable that is initialized to start_value and updated on each iteration.
- start_value: The initial value of the control variable.
- end_value: The final value of the control variable. The loop continues as long as the control variable is less than or equal to the end_value.
- step_value: (Optional) The value by which the control variable is incremented on each iteration. If not specified, the default step value is 1.
- record: A record variable that is iterated over in the FOR EACH loop.
- table: The table or set of records to iterate through.

## Iterating Over a Range

The FOR loop that iterates over a range is commonly used for executing a block of code a specific number of times. The control variable is initialized to start_value and is incremented by step_value (default is 1) on each iteration. The loop continues to execute until the control variable exceeds end_value.

## Step Value

The STEP clause is optional and allows you to specify the increment for the control variable on each iteration. By default, the control variable is incremented by 1. If you need to skip certain values or decrement, you can use the STEP clause to control this behavior.

## Iterating Through a Set of Records

The FOR EACH loop is specifically designed for iterating through records in a database table. It allows you to perform operations on each record within the specified table. The loop variable (record) is assigned each record in the table sequentially, and the block of code within the loop is executed for each record.

## Nested FOR Loops

Nested FOR loops are used when you need to perform multi-level iterations. For example, you might want to iterate over rows and columns in a grid or matrix. The inner loop completes all its iterations for each single iteration of the outer loop, providing a comprehensive way to handle multi-dimensional data or complex sequences.

