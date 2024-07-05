## Storage Formats of Data Types

### Character Types

#### char / character
- Range: 1 to 32000 bytes (1 to 16000 bytes in a UTF-8 instance)
- Description: Fixed-length character string

#### varchar
- Range: 1 to 32000 bytes (1 to 16000 bytes in a UTF-8 instance)
- Description: Variable-length character string

#### long varchar
- Range: 1 to 2 GB bytes
- Description: Long variable-length character string

#### nchar / nvarchar (Unicode)
- Range: 1 to 16,000 characters (32,000 bytes)
- Description: Unicode character string (fixed-length for nchar, variable-length for nvarchar)

### Integer Types

#### integer1 (tinyint)
- Range: -128 to +127
- Description: 1-byte integer

#### smallint (integer2)
- Range: -32,768 to +32,767
- Description: 2-byte integer

#### integer (integer4)
- Range: -2,147,483,648 to +2,147,483,647
- Description: 4-byte integer

#### integer8 (bigint)
- Range: -9,223,372,036,854,775,808 to +9,223,372,036,854,775,807
- Description: 8-byte integer

### Decimal and Floating-Point Types

#### decimal
- Range: Depends on precision and scale
- Default: (5,0) - Range from -99999 to +99999
- Maximum number of digits: 39
- Description: Fixed-point exact numeric

#### float4
- Range: -1.0e+38 to +1.0e+38
- Precision: 7 digits
- Description: 4-byte floating-point number

#### float (float8)
- Range: -1.0e+38 to +1.0e+38
- Description: 8-byte floating-point number

### Date and Time Types

#### date(ingresdate)
- Size: 12 bytes
- Range for absolute dates: 1-jan-0001 to 30-dec-9999
- Range for time intervals: -9999 years to +9999 years

### Money Type

#### money
- Size: 8 bytes
- Range: $-999,999,999,999.99 to $999,999,999,999.99

### Binary Type

#### long byte
- Range: 1 to 2 GB of binary data
- Description: Large binary object storage
