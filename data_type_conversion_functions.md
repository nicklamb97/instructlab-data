# Data Type Conversion Functions

OpenROAD provides the following data type conversion functions:

- **c(expr)**
  - Operand Type: any
  - Result Type: c
  - Description: Converts any value to a c string

- **char(expr)**
  - Operand Type: any
  - Result Type: char
  - Description: Converts any value to a char string

- **date(expr)**
  - Operand Type: c, char, text, varchar
  - Result Type: date
  - Description: Converts a c, char, varchar, or text string to internal date representation

- **decimal(expr, 1 < p <31, 0 < s <p)**
  - Operand Type: c, char, varchar, text, float, money, integer(1), smallint, integer
  - Result Type: decimal
  - Description: Returns the decimal representation of the argument string

- **dow(expr)**
  - Operand Type: date
  - Result Type: c
  - Description: Converts an absolute date into its day of week (for example, 'Mon,' 'Tue'); the result length is 3

- **float4(expr)**
  - Operand Type: c, char, varchar, text, float, money, integer(1), smallint, integer
  - Result Type: float4
  - Description: Converts the specified expression to float4

- **float8(expr)**
  - Operand Type: c, char, varchar, text, float, money, integer(1), smallint, integer
  - Result Type: float
  - Description: Converts the specified expression to float

- **hex(expr)**
  - Operand Type: varchar, c, char, text
  - Result Type: varchar
  - Description: Returns the hexadecimal representation of the argument string; the length of the result is twice the length of the argument, because the hexadecimal equivalent of each character requires two bytes. For example, hex('A') returns '61' (ASCII) or 'C1' (EBCDIC).

- **int1(expr)**
  - Operand Type: c, char, varchar, text, float, money, integer(1), smallint, integer
  - Result Type: integer1
  - Description: Converts the specified expression to integer1; floating point values are truncated

- **int2(expr)**
  - Operand Type: c, char, varchar, text, float, money, integer(1), smallint, integer
  - Result Type: smallint
  - Description: Converts the specified expression to smallint; floating point values are truncated

- **int4(expr)**
  - Operand Type: c, char, varchar, text, float, money, integer(1), smallint, integer
  - Result Type: integer
  - Description: Converts the specified expression to integer; floating point values are truncated

- **int8(expr)**
  - Operand Type: c, char, varchar, text, float, money, decimal, integer1, smallint, integer, boolean
  - Result Type: bigint
  - Description: Converts the expression to bigint. Decimal, floating point, and string values are truncated by discarding the fractional portion of the argument. Numeric overflow occurs if the argument is too large for the result type.

- **money(expr)**
  - Operand Type: c, char, varchar, text, float, integer(1), smallint, integer, money
  - Result Type: money
  - Description: Converts the specified expression to internal money representation; rounds floating point values, if necessary

- **nvarchar(expr [, len])**
  - Operand Type: any
  - Result Type: nvarchar
  - Description: Converts argument to nvarchar Unicode string. If the optional length argument is specified, the function returns the leftmost len characters. Len must be a positive integer value. If len exceeds the length of the expr string, the varying length is set to match the character length of the expr.

- **text(expr)**
  - Operand Type: any
  - Result Type: text
  - Description: Converts any value to a text string; this function removes any trailing blanks from c or char string expressions

- **to_char(datetime [,format])**
  - Operand Type: ansidate, timestamp, timstamp with time zone, or timstamp with local time zone
  - Result Type: varchar
  - Description: Converts a datetime or interval value to a value of varchar type in the specified date format. If you omit format, then date is converted to a varchar value as follows:
    - Ansidate values are converted to values in the default date format.
    - Timestamp and timestamp with local time zone values are converted to values in the default timestamp format.
    - Timestamp with time zone values are converted to values in the default timestamp with time zone format.

  Format can be:
  - punctuation "text"--Preserves literal punctuation and quoted text
  - AD, A.D--AD indicator
  - AM, A.M., PM, P.M.--AM/PM indicator
  - CC--Century
  - D--Number of day of week 1 - 7
  - DAY--Day name padded to suitable width to retain column
  - DD--Day of month 1 - 31
  - DDD--Day of year 1 - 366
  - DY--Day name in 3 characters
  - E, EE--Era name
  - FF [1...9]--Fractional seconds
  - FM--Toggle whether formats have leading or trailing blanks
  - FX--Toggle whether exact matching is required
  - HH--Hour od day 1 - 12
  - HH12--Hour of day 1 - 12
  - HH24--Hour of day 0 - 23
  - IW--ISO week of year 1 - 53
  - IYYY, IYY, IY, I--Last 4, 3, 2, 1 digits of ISO year
  - J--Julian day (days since January 1, 4712 BC)
  - MI--Minutes 0 - 59
  - MM--Month 01 - 12
  - MON--First 3 characters of month name
  - MONTH--Month name padded to suitable length
  - Q--Quarter 1 - 4
  - RM--Month in Roman numerals
  - RR--Round year in 2 digits
  - RRRR--Round year in 2 or 4 digits
  - SS--Seconds since midnight 1 - 86399
  - TZH--Time zone hour
  - WW--Week of year 1 - 53
  - W--Week of month 1 - 5
  - X--Decimal
  - Y,YYY--Year with comma in this position
  - YEAR--Year spelled out
  - YYYY, YYY, YY, Y--Last 4, 3, 2, or 1 digits of year

  The case and punctuation of the format are important. Formats whose results are alphabetic follow the case of the format. For example, 'DY' will yield 'MON' but 'Dy' will yield 'Mon'.

  ```sql
  SELECT TO_CHAR('2013-07-30 23:42:00.290533-07:00','YEAR-MONTH-DAY HH24:MI:SSXFF6 tzh:tzm');
  ```
  returns 2013-JULY-TUESDAY 23:42:00.290533 -07:00

  ```sql
  SELECT TO_CHAR(SYSDATE,'YYYY-MON-DD');
  ```
  returns 2013-JUL-30

  ```sql
  SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI:SS');
  ```
  returns 2013-07-30 23:36:19

- **unhex(expr)**
  - Operand Type: any
  - Result Type: varchar
  - Description: Returns the opposite of the hex function. For example, unhex('61626320') returns 'abc ' and unhex('01204161') returns '\001Aa'.
    Exceptions can occur when a "c" data type suppresses the display of certain stored characters, or when the output data type differs from the input type.
    Normally one character is generated for every two hex digits being converted to a printable character. If the hex digit pair being converted does not translate to a printable character, the value is converted to a backslash (\), followed by the numeric value of the hex digit pair as a three-digit octal value.

- **varchar(expr)**
  - Operand Type: any
  - Result Type: varchar
  - Description: Converts any value to a varchar string; this function removes any trailing blanks from c or char string expressions
