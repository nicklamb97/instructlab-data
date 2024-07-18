# Numeric Functions

SQL supports the following numeric functions:

- **ABS(n)**
  - Operand Type: all numeric types and money
  - Result Type: same as n
  - Description: Absolute value of n.

- **ACOS(n)**
  - Operand Type: all numeric types
  - Result Type: float
  - Description: Arccosine of cosine value n

- **ASIN(n)**
  - Operand Type: all numeric types
  - Result Type: float
  - Description: Arcsine value of sine value n

- **ATAN(n)**
  - Operand Type: all numeric types
  - Result Type: float
  - Description: Arctangent of n; returns a value from (-pi/2) to pi/2.

- **ATAN2 (x, y)**
  - Operand Type: all numeric types
  - Result Type: float
  - Description: Arctangent of angle defined by coordinate pair (x, y)

- **CEIL(n)** or **CEILING(n)**
  - Operand Type: all numeric types
  - Result Type: decimal
  - Description: Returns the smallest integer greater than or equal to the specified numeric expression.

- **COS(n)**
  - Operand Type: all numeric types
  - Result Type: float
  - Description: Cosine of n; returns a value from -1 to 1.

- **EXP(n)**
  - Operand Type: all numeric types and money
  - Result Type: float
  - Description: Exponential of n.

- **FLOOR(n)**
  - Operand Type: all numeric types
  - Result Type: decimal
  - Description: Returns the largest integer less than, or equal to, the specified numeric expression.

- **LOG(n)** or **LN(n)**
  - Operand Type: all numeric types and money
  - Result Type: float
  - Description: Natural logarithm of n.

- **MOD(n,b)**
  - Operand Type: integer, smallint, integer1, decimal
  - Result Type: same as b
  - Description: n modulo b. The result is the same data type as b. Decimal values are truncated.

- **PI()**
  - Operand Type: None
  - Result Type: float
  - Description: Value of pi (ratio of the circumference of a circle to its diameter)

- **ROUND(n,i)**
  - Operand Type: all numeric types
  - Result Type: decimal
  - Description: Rounds value at the i'th place right or left of the decimal, depending on whether i is greater or less than 0.

- **SIGN(n)**
  - Operand Type: all numeric types
  - Result Type: integer
  - Description: -1 if n < 0, 0 if n = 0, +1 if n > 0

- **SIN(n)**
  - Operand Type: all numeric types
  - Result Type: float
  - Description: Sine of n; returns a value from -1 to 1.

- **SQRT(n)**
  - Operand Type: all numeric types and money
  - Result Type: float
  - Description: Square root of n.

- **TAN(n)**
  - Operand Type: all numeric types
  - Result Type: float
  - Description: Tangent value of angle n

- **TRUNC(x ,y)**
  - Operand Type: all numeric types
  - Result Type: decimal
  - Description: Truncates x at the decimal point, or at y places to the right or left of the decimal, depending on whether y is greater or less than 0.

For trigonometric functions (COS, SIN, TAN), specify argument in radians. To convert degrees to radians, use the following formula:

```
radians = degrees / 360 * 2 * pi()
```

The functions ACOS, ASIN, ATAN, AND ATAN2 return a value in radians.
