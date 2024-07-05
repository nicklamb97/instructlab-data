## Numeric Data Types

Numeric data types fall into two categories: exact and approximate. 

### Exact Numeric Data Types

#### Integer Data Types

Exact numeric data types include the following integer types:

- **integer1 (tinyint)**: One-byte integer
  - Range: -128 to +127
- **smallint (integer2)**: Two-byte integer
  - Range: -32,768 to +32,767
- **integer (integer4)**: Four-byte integer
  - Range: -2,147,483,648 to +2,147,483,647
- **integer8 (bigint)**: Eight-byte integer
  - Range: -9,223,372,036,854,775,808 to +9,223,372,036,854,775,807

#### Decimal Data Types

The decimal data type is an exact numeric type defined by its precision (total number of digits) and scale (number of digits to the right of the decimal point).

- Minimum precision: 1
- Maximum precision: 39
- Scale: Cannot exceed precision (can be 0)

**Syntax**: `decimal(p,s)` where `p` is precision and `s` is scale.

**Note**: Synonyms for decimal are `dec` and `numeric`.

**Usage**: Suitable for storing currency data where required range or precision exceeds the money data type. However, it doesn't include a currency sign for display purposes.

### Approximate Numeric Data Types

#### Floating Point Data Types

Floating point values can be represented as whole plus fractional digits or as a mantissa plus an exponent.

Two types of floating point data:

1. **float4 (real)**: 4-byte floating point
2. **float (float8, double precision)**: 8-byte floating point

**Precision**:
- 8-byte numbers are rounded to 15 decimal digits
- 4-byte number precision is processor-dependent

**Optional Syntax**: `float(n)` where n is binary precision (0-53)
- 0 to 23: 4-byte float
- 24 to 53: 8-byte float

### Important Considerations

1. **Data Type Conversions**: Be cautious when combining or comparing numeric values, especially with floating point numbers.
2. **Floating Point Limitations**: 
   - Exact matches on floating point numbers are discouraged due to their approximate nature.
   - Float and float4 are approximate numeric values.
   - Integer and decimal are exact numeric values.
3. **Decimal for Currency**: The decimal data type is preferable for currency calculations where precision is crucial.
4. **Storage**: Floating point numbers are stored in four or eight bytes, depending on the type and specified precision.

Remember to choose the appropriate numeric data type based on your specific requirements for range, precision, and exactness of calculations.
