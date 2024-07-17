Remove Dbevent Statement
The remove dbevent statement removes a database event for which an application has previously registered.
This statement has the following syntax:
remove dbevent [schema.]event_name;
The remove dbevent statement specifies that an application no longer intends to receive the specified database event.
If the database event has been raised before the application removes the registration, the database event remains queued to the application and will be received when the application issues the get dbevent statement (see the Ingres SQL Reference Guide).
Parameters--Remove Dbevent Statement
This statement has the following parameter:
event_name
Specifies the name of the database event to be removed
