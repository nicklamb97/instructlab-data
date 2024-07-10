## Named Parameter List Without Byref

Specifies values that are passed as parameters with parameter names. The values are specified as a series of assignments separated by commas, each linking a parameter with an expression.

This element has the following syntax:

*parameter* **=** *expression*{**,** *parameter* **=** *expression*}

### *parameter*

Specifies the name of a parameter in the called frame to which you are passing a value from the calling frame, procedure, or method. It is a dynamic name.

### *expression*

Specifies any legal 4GL expression that evaluates to the appropriate data type
