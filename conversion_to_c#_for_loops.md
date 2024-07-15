# FOR Loops in Actian 4GL for OpenROAD

This example illustrates how nested FOR loops work in Actian 4GL. The outer loop iterates over the range from 1 to 5, and for each value of i, the inner loop iterates over the range from 1 to 5. The result is a series of displayed lines that show the values of i and j and their product, effectively creating a multiplication table.

```4gl
MESSAGE "Example 4: Nested FOR Loops";
FOR i = 1 TO 5
    DEFINE j INTEGER;
    FOR j = 1 TO 5
        MESSAGE i * j;
    ENDFOR;
ENDFOR;
```

## Code Breakdown

### Initial MESSAGE Statement:
- `MESSAGE "Example 4: Nested FOR Loops";`:
  - This statement outputs the text "Example 4: Nested FOR Loops" to indicate the start of this example in the output.

### Outer FOR Loop:
- `FOR i = 1 TO 5`:
  - This loop initializes the control variable i to 1 and will continue to iterate as long as i is less than or equal to 5.
  - The value of i is incremented by 1 after each iteration of the loop.

### Inner FOR Loop:
- `DEFINE j INTEGER;`:
  - This statement defines a new integer variable j that will be used as the control variable for the inner loop.
- `FOR j = 1 TO 5`:
  - This loop initializes the control variable j to 1 and will continue to iterate as long as j is less than or equal to 5.
  - The value of j is incremented by 1 after each iteration of the loop.

### MESSAGE Statement Inside Inner Loop:
- `MESSAGE i * j;`:
  - This statement outputs the current values of i and j, and their product (i * j), to the display.
  - The output format will be: "<product of i and j>".

### Execution Flow:
- The outer loop starts with i = 1.
  - The inner loop starts with j = 1 and displays the product of i and j.
  - The inner loop increments j to 2 and displays the product of i and j.
  - This process repeats until j exceeds 5.
- The outer loop increments i to 2 and repeats the inner loop process for j from 1 to 5.
- This nested iteration continues until i exceeds 5.

# Equivalent FOR Loops in C# 

This example illustrates how nested for loops work in C#. The outer loop iterates over the range from 1 to 5, and for each value of i, the inner loop iterates over the range from 1 to 5. The result is a series of displayed lines that show the values of i and j and their product, effectively creating a multiplication table.

```csharp
Console.WriteLine("Example 4: Nested FOR Loops");
for (int i = 1; i <= 5; i++)
{
    for (int j = 1; j <= 5; j++)
    {
        Console.WriteLine($"i: {i}, j: {j} - Product: {i * j}");
    }
}
```

## Code Breakdown

### Initial Output Statement:
- `Console.WriteLine("Example 4: Nested FOR Loops");`:
  - This statement outputs the text "Example 4: Nested FOR Loops" to indicate the start of this example in the output.

### Outer For Loop:
- `for (int i = 1; i <= 5; i++)`:
  - This loop initializes the control variable i to 1 and will continue to iterate as long as i is less than or equal to 5.
  - The value of i is incremented by 1 after each iteration of the loop.

### Inner For Loop:
- `for (int j = 1; j <= 5; j++)`:
  - This loop initializes the control variable j to 1 and will continue to iterate as long as j is less than or equal to 5.
  - The value of j is incremented by 1 after each iteration of the loop.

### Output Statement Inside Inner Loop:
- `Console.WriteLine($"i: {i}, j: {j} - Product: {i * j}");`:
  - This statement outputs the current values of i and j, and their product (i * j), to the display.
  - The output format will be: i: <value of i>, j: <value of j> - Product: <product of i and j>.

### Execution Flow:
- The outer loop starts with i = 1.
  - The inner loop starts with j = 1 and displays the product of i and j.
  - The inner loop increments j to 2 and displays the product of i and j.
  - This process repeats until j exceeds 5.
- The outer loop increments i to 2 and repeats the inner loop process for j from 1 to 5.
- This nested iteration continues until i exceeds 5.
