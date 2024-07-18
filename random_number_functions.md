# Random Number Functions

The random number function is used to generate random values. Use the following statement to set the beginning value for the random functions:

```
[EXEC SQL] SET RANDOM_SEED [value]
```

The seed value can be any integer. There is a global seed value and local seed values. The global value is used until you issue SET RANDOM_SEED, which changes the value of the local seed. Once changed, the local seed is used for the whole session. If you are using the global seed value, the seed is changed whenever a random function executes. This means that other users issuing random calls enhance the "randomness" of the returned value.

If you omit the value, Ingres multiplies the process ID by the number of seconds past 1/1/1970 until now. This value generates a random starting point. You can use *value* to run a regression test from a static start and get identical results.

The random number functions are:

**RAND() or RAND(*integer*)**

Result type: float4

Returns a random floating point value between 0.0 and 1.0. If the optional integer parameter is specified, the value is used as a random seed. This function is the same as the RANDOM() function.

**RANDOM()**

Returns a random integer based on a seed value.

**RANDOMF()**

Returns a random float based on a seed value between 0 and 1. This is slower than RANDOM, but produces a more random number.

**RANDOM(l,h)**

Returns a random integer within the specified range (that is, l >= x <= h).

**RANDOMF(l,h)**

Passing two integer values generates an integer result within the specified range; passing two floats generates a float within the specified range; passing an int and a float causes them to be coerced to an int and generates an integer result within the specified range (that is, l >= x <= h).

**SRAND(*integer*)**

Sets the random seed using the specified integer value. Always returns 0. It performs the same function as the SET RANDOM_SEED statement.
