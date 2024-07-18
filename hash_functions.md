# HASH Functions

The HASH function is used to generate a four-byte numeric value from expressions of all data types except long data types. Note that the implicit size for the expression can affect the result. For example:

```
SELECT HASH(int1(1)), HASH(int2(1)), HASH(int4(1))\g
```

returns the following single row:

**Col2**

**Col3**

**Col4**

1526341860

-920527466

-1447292811

**Note:** Constant values such as HASH(1) will return a value based on the implicit datatype of the constant. For Ingres this is equivalent to HASH(int2(1)). For OpenROAD this is equivalent to HASH(int4(1)).
