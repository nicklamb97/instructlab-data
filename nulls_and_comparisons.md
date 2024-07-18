# Nulls and Comparisons

Because a null is not a value, it cannot be compared to any other value (including another null value). For example, the following condition evaluates to false if one or both of the variables is null:

```
if columna = columnb
```

Similarly, the where clause:

```
if columna < 10 or columna >= 10
```

is true for all numeric values of columna, but false if columna is null.
