Raise Dbevent Statement
The raise dbevent statement enables an application to notify other applications of its status.
This statement has the following syntax:
raise dbevent [schema.]event_name [event_text]
               [with [no]share];
Parameters--Raise Dbevent Statement
This statement has the following parameters:
event_name
Specifies an existing database event name
event_text
Specifies a string (maximum 256 character) to receiving applications. To obtain the text, receiving applications must use the inquire_sql(variable = MESSAGETEXT) statement.
Raise Dbevent Description
The raise dbevent statement enables a session to communicate status information to other sessions that are registered to receive event_name.
If schema is omitted, the DBMS Server checks first for the specified database event owned by the effective user of the session. If the current effective user does not own the database event, the DBMS Server seeks the specified database event in the database events owned by the DBA.
Use the optional event_text parameter to pass a (maximum 256 character) string to receiving applications; to obtain the text, receiving applications must use the inquire_sql(variable = MESSAGETEXT) statement.
To restrict database event notification to the session that raised the database event, specify with noshare. To notify all registered sessions, specify with share or omit this clause. The default is share.
If a database event is raised from within a transaction and the transaction is subsequently rolled back, the database event notification is not rolled back.
Raise Dbevent Permissions
To raise a database event you do not own, specify the schema parameter and have RAISE privilege for the database event. To assign RAISE privilege to another user, use the grant statement (see Grant (privilege) Statement).
Raise Dbevent Related Statements
Create Dbevent Statement
Register Dbevent Statement
