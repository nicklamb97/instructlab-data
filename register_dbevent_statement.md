Register Dbevent Statement
The register dbevent statement enables a session to specify the database events it intends to receive.
This statement has the following syntax:
register dbevent [schema.]event_name;
A session receives only the database events for which it has registered. To remove a registration, use the remove statement.
A session can register for events owned by the session's effective user or for which register privilege has been granted. If an attempt is made to register for a nonexistent event, for an event for which register privilege has not been granted, or twice for the same event, the DBMS Server issues an error.
If the schema parameter is omitted, the DBMS Server first checks the events owned by the current user. If the specified event is not found, the DBMS Server checks the events owned by the DBA.
If the register dbevent statement is issued from within a transaction that is subsequently rolled back, the registration remains in effect.
Parameters--Register Dbevent Statement
This statement has the following parameter:
event_name
Specifies the database event name to register
Register Dbevent Permissions
To register for an event you do not own, you must specify the schema parameter, and must have REGISTER privilege for the database event. To assign REGISTER privilege, use the grant statement (see Grant (privilege) Statement).
Register Dbevent Related Statements
Create Dbevent Statement
Raise Dbevent Statement
