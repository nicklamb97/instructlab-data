# Comparison of Actian 4GL and C# Code Examples

This document provides a side-by-side comparison of Actian 4GL and C# code examples for various scenarios.
OpenROAD, which uses Actian 4GL, is a powerful application development tool that can be compared to C# in certain aspects.

## Example 1: Age Classification

### Actian 4GL
```4gl
DEFINE age INTEGER
LET age = 20
IF age < 18 THEN
DISPLAY "You are a minor."
ELSE IF age >= 18 AND age < 65 THEN
DISPLAY "You are an adult."
ELSE
DISPLAY "You are a senior."
END IF
```

### C#
```csharp
int age = 20;
if (age < 18)
{
    Console.WriteLine("You are a minor.");
}
else if (age >= 18 && age < 65)
{
    Console.WriteLine("You are an adult.");
}
else
{
    Console.WriteLine("You are a senior.");
}
```

Both examples check the age variable and print a message depending on the age range.

In Actian 4GL (used in OpenROAD), DISPLAY is used to show the message.
In C#, Console.WriteLine is used for displaying the message.

## Example 2: Temperature Classification
### Actian 4GL
```4gl
DEFINE temperature DECIMAL(5,2)
LET temperature = 72.5
IF temperature < 60 THEN
    DISPLAY "It's cold."
ELSE IF temperature >= 60 AND temperature < 80 THEN
    DISPLAY "It's pleasant."
ELSE
    DISPLAY "It's hot."
END IF
```

### C#
```csharp
decimal temperature = 72.5M;
if (temperature < 60)
{
    Console.WriteLine("It's cold.");
}
else if (temperature >= 60 && temperature < 80)
{
    Console.WriteLine("It's pleasant.");
}
else
{
    Console.WriteLine("It's hot.");
}
```

Both examples check the temperature variable and print a message based on its value.

In Actian 4GL, the temperature is of type DECIMAL.
In C#, the temperature is of type decimal, denoted with an M.

## Example 3: Grade Evaluation
### Actian 4GL
```4gl
DEFINE grade CHAR(1)
LET grade = 'B'
IF grade = 'A' THEN
    DISPLAY "Excellent!"
ELSE IF grade = 'B' THEN
    DISPLAY "Good!"
ELSE IF grade = 'C' THEN
    DISPLAY "Average."
ELSE IF grade = 'D' THEN
    DISPLAY "Below Average."
ELSE
    DISPLAY "Failing."
END IF
```

C#
```csharp
char grade = 'B';
if (grade == 'A')
{
    Console.WriteLine("Excellent!");
}
else if (grade == 'B')
{
    Console.WriteLine("Good!");
}
else if (grade == 'C')
{
    Console.WriteLine("Average.");
}
else if (grade == 'D')
{
    Console.WriteLine("Below Average.");
}
else
{
    Console.WriteLine("Failing.");
}
```

Both examples evaluate a grade and print a corresponding message.

In Actian 4GL, the grade is of type CHAR.
In C#, the grade is of type char.

## Example 4: Income Classification
### Actian 4GL
```4gl
DEFINE salary DECIMAL(10,2)
LET salary = 55000.00
IF salary < 30000 THEN
    DISPLAY "Low income."
ELSE IF salary >= 30000 AND salary < 70000 THEN
    DISPLAY "Middle income."
ELSE
    DISPLAY "High income."
END IF
```

C#
```csharp
decimal salary = 55000.00M;
if (salary < 30000)
{
    Console.WriteLine("Low income.");
}
else if (salary >= 30000 && salary < 70000)
{
    Console.WriteLine("Middle income.");
}
else
{
    Console.WriteLine("High income.");
}
```

Both examples evaluate a salary and print a message based on the income range.

In Actian 4GL, the salary is of type DECIMAL.
In C#, the salary is of type decimal, denoted with an M.

## Example 5: Discount Calculation
### Actian 4GL
```4gl
DEFINE totalPurchase DECIMAL(10,2)
LET totalPurchase = 120.50
IF totalPurchase < 50 THEN
    DISPLAY "No discount."
ELSE IF totalPurchase >= 50 AND totalPurchase < 100 THEN
    DISPLAY "5% discount."
ELSE
    DISPLAY "10% discount."
END IF
```

C#
```csharp
decimal totalPurchase = 120.50M;
if (totalPurchase < 50)
{
    Console.WriteLine("No discount.");
}
else if (totalPurchase >= 50 && totalPurchase < 100)
{
    Console.WriteLine("5% discount.");
}
else
{
    Console.WriteLine("10% discount.");
}
```

Both examples evaluate the totalPurchase amount and print a message based on discount eligibility.

In Actian 4GL, the total purchase is of type DECIMAL.
In C#, the total purchase is of type decimal, denoted with an M.