## Data Types

When you explicitly declare a variable, you must assign a data type to it. A simple variable can be any of the following supported data types:

### Character Types

#### char / character
- OpenROAD Intrinsic Data Type: char
- OpenROAD Built-in Scalar Object: StringObject
- Client-side Coercion: yes
- Server-side Coercion: yes

#### varchar / character varying
- OpenROAD Intrinsic Data Type: varchar
- OpenROAD Built-in Scalar Object: StringObject
- Client-side Coercion: yes
- Server-side Coercion: yes

#### long varchar / clob / character large object / char large object
- OpenROAD Intrinsic Data Type: not supported
- OpenROAD System Class: LongVcharObject
- Client-side Coercion: no
- Server-side Coercion: no

#### nchar / nvarchar
- OpenROAD Intrinsic Data Type: nchar* / nvarchar*
- OpenROAD Built-in Scalar Object: StringObject
- Client-side Coercion: yes
- Server-side Coercion: yes

### Numeric Types

#### integer / integer4
- OpenROAD Intrinsic Data Type: integer, integer4
- OpenROAD Built-in Scalar Object: IntegerObject
- Client-side Coercion: yes
- Server-side Coercion: yes

#### smallint / integer2
- OpenROAD Intrinsic Data Type: smallint, integer2
- OpenROAD Built-in Scalar Object: IntegerObject
- Client-side Coercion: yes
- Server-side Coercion: yes

#### bigint / integer8
- OpenROAD Intrinsic Data Type: integer8
- OpenROAD Built-in Scalar Object: none (for bigint), IntegerObject (for integer8)
- Client-side Coercion: yes
- Server-side Coercion: yes

#### decimal
- OpenROAD Intrinsic Data Type: decimal
- OpenROAD Built-in Scalar Object: DecimalObject
- Client-side Coercion: yes
- Server-side Coercion: yes

#### float / float8 / double precision
- OpenROAD Intrinsic Data Type: float, float8
- OpenROAD Built-in Scalar Object: FloatObject
- Client-side Coercion: yes
- Server-side Coercion: yes

### Date and Time Types

#### date
- OpenROAD Intrinsic Data Type: date
- OpenROAD Built-in Scalar Object: DateObject
- Client-side Coercion: yes
- Server-side Coercion: yes

#### timestamp (including with/without time zone)
- OpenROAD Intrinsic Data Type: not supported
- Client-side Coercion: yes**
- Server-side Coercion: yes

### Other Types

#### money
- OpenROAD Intrinsic Data Type: money
- OpenROAD Built-in Scalar Object: MoneyObject
- Client-side Coercion: yes
- Server-side Coercion: yes

#### boolean
- OpenROAD Intrinsic Data Type: not supported
- Client-side Coercion: no
- Server-side Coercion: yes

*Note on nchar and nvarchar: OpenROAD transparent Unicode support negates the need for these data types in OpenROAD 4GL applications. The varchar data type with UTF8 encoding is sufficient for Unicode support.

**Loss of precision may occur

For more detailed information about specific data types, including length and storage formats, refer to the Storage Formats of Data Types documentation. If using Enterprise Access, consult the Enterprise Access documentation for data type specifics.

A reference or array variable can be any named user class or system class. For information about creating a named user class, see the Workbench User Guide.

By default, all data types are nullable. You can use the `not null` clause to declare a data type non-nullable. However, a class data type is always nullable.
