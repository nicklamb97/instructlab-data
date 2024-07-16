Direct Execute Immediate Statement
The direct execute immediate statement sends DBMS‑specific commands to the DBMS without translation.
This statement has the following syntax:
direct execute immediate string | string_variable;
The direct execute immediate statement allows statements to be sent to the Enterprise Access product or DBMS to which a session is connected. The Enterprise Access product does not translate the statement. If the statement is not supported by the DBMS or Enterprise Access product, an error is returned. The direct execute immediate statement cannot be used to return rows to a session.
A host language variable or string literal can be used to specify the statement. If you use a string literal, avoid embedding quotes in the literal. If you specify the statement using a host language variable, the OpenSQL string‑delimiting conventions must be observed.
Parameters--Direct Execute Immediate Statement
This statement has the following parameters:
string
Specifies a string literal
string_variable
Specifies a host language variable
