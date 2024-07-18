# Direct Execute Immediate Statement

The direct execute immediate statement sends DBMS-specific commands to the DBMS without translation.

## Syntax

```
direct execute immediate string | string_variable;
```

The `direct execute immediate` statement allows sending statements directly to the Enterprise Access product or DBMS connected to the session. The statement is not translated by the Enterprise Access product. If the DBMS or Enterprise Access product does not support the statement, an error is returned. This statement cannot be used to return rows to a session.

A host language variable or string literal can be used to specify the statement. If using a string literal, avoid embedding quotes within the literal. When specifying the statement using a host language variable, adhere to OpenSQL string-delimiting conventions.

## Parameters

- **string**: Specifies a string literal representing the statement to be executed.
- **string_variable**: Specifies a host language variable containing the statement to be executed.
