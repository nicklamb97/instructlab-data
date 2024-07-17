Set Statement
The set statement specifies a runtime option for the current session. The selected option remains in effect until the session is terminated or the option is changed using another set statement.
Note:  This statement has additional considerations when used in a distributed environment. For more information, see the Ingres Star User Guide.
This statement has the following syntax:
set autocommit on | off
set [no]batch_copy_optim
set [no]cache_dynamic
set [no]cardinality_check
set cpufactor [value]
set date_format [value]
set decimal [value]
set [no]flatten
set [no]hash
set [no]io_trace
set joinop notimeout | timeout | timeout 'nnn'
set joinop timeoutabort 'nnn'
set joinop [no]greedy
set nojournaling | journaling [on table_name]
set lockmode session|on table_name where
              [level = page | table | session | system | row | mvcc]
              [, readlock = nolock | shared | exclusive
                               | session | system]
              [, maxlocks = n | session | system]
              [, timeout = n | session | system | nowait]
set [no]lock_trace
set [no]logdbevents
set [no]logging
set [no]log_trace
set nomaxconnect | maxconnect value
set nomaxcost | maxcost value
set nomaxcpu | maxcpu value
set nomaxidle | maxidle value
set nomaxio | maxio value
set nomaxpage | maxpage value
set nomaxquery | maxquery value
set nomaxrow | maxrow value
set money_format [value]
set money_prec [value]
set [no]ojflatten
set [no]optimizeonly
set [no]parallel [degree of parallelism]
set [no]printdbevents
set [no]printqry
set [no]printrules
set [no]qep
set random_seed [value]
set result_structure
                heap | cheap | heapsort | cheapsort | hash | chash
               | isam | cisam | btree | cbtree
set role none | rolename [with password = 'pwd'];
set [no]rules
set session
                [with
                      on_error = rollback statement | transaction
                      on_user_error = rollback transaction | norollback
set session authorization username | user | current_user
                   session_user | system_user | initial_user
set [no]statistics tablename
set notrace output | trace output filename
set [no]trace point [value]
 
set update_rowcount changed | qualified
Parameters--Set Statement
Parameters for the various set options are detailed in the following sections.
Set Permissions
To issue the following statements, a user must have TRACE privilege:
•set [no]lock_trace
•set [no]printqry
•set [no]rules
•set [no]printrules
•set [no]printdbevents
•set [no]logdbevents
•set [no]io_trace
•set [no]log_trace
•set [no]trace point value
To issue the set work locations statement, the effective user of the session must have MAINTAIN_LOCATIONS privilege. For more information see CREATE USER and ALTER USER in the Ingres SQL Reference Guide.
To issue the following statements, you must be the DBA of the database to which the session is attached:
•set [no]rules
•set [no]logging
To issue the set lockmode statement, the effective user of the session must have LOCKMODE privilege. LOCKMODE privilege is assigned using the Grant (privilege) Statement.
Autocommit
The set autocommit on statement directs the DBMS Server to treat each query as a single-query transaction. Set autocommit off, the default, means an explicit commit or rollback statement or terminating the session is required to terminate a transaction.
The set autocommit statement cannot be issued in an open transaction. For a description multi-statement transaction behavior, see the chapter “Working with Transactions and Handling Errors.”
[No]Batch_Copy_Optim
Set batch_copy_optim turns on copy optimization when executing batched inserts. This statement overrides the default set on the DBMS Server batch_copy_optim configuration parameter.
Set nobatch_copy_optim turns off copy optimization.
[No]Cache_dynamic
Set [no]cache_dynamic enables or disables the caching of query plans for cursors defined with dynamic select statements. It overrides the server level setting defined by the cache_dynamic configuration parameter. To check the current setting, use select dbmsinfo ('cache_dynamic').
The set [no]cache_dynamic setting persists until the DBMS Server is shut down.
See also Session [No]Cache_Dynamic.
[No]Cardinality_check
Set [no]cardinality_check lets you control the where clause singleton subselect cardinality checking.
In previous releases that supported where clause subqueries, a cardinality error (that is, a result contained too many results) was not raised unless set noflatten was specified. With the introduction of scalar subqueries, cardinality errors may be reported that were ignored previously.
Set cardinality_check raises the cardinality violation exception and otherwise restores the legacy behavior. The session default setting is derived from a CBF parameter ii.nodename.dbms.*.cardinality_check, which defaults to ON.
Cpufactor
If you are working on a machine where CPU operations are more expensive than I/O operations, the default cpufactor can be set to a lower value. This causes the Optimizer to look at less CPU-intensive operations.
The default is 100 CPU operations to one disk I/O.
For example: Setting cpufactor to 10 indicates that one disk I/O is roughly equivalent to 10 CPU units.
Date_format
Set date_format specifies the format for date values. This option corresponds to the Ingres environment variable II_DATE_FORMAT and is assigned the same values. If set, date_format replaces the currently configured format with an alternate format. The default format setting is US. See the Workbench User Guide for a list of valid settings.
To set the date_format from within a 4GL routine, you must pass the set date_format a variable containing the desired format with single quotes before and after the date format string. For example:
initialize()=
declare
          df = varchar(20) not null;
enddeclare
{
          df = '''MULTINATIONAL''';
          set date_format :df;
}
Decimal
Set decimal specifies the character to be used as the decimal point in numeric literals. This option corresponds to the Ingres environment variable II_DECIMAL and is assigned the same values. Valid characters are the period (.) (as in 12.34) and the comma (,) (as in 12,34). The default is the period. See the Workbench User Guide for more information about setting this option.
To set the decimal character from within a 4GL routine, you must pass to the set decimal a variable containing the desired format with single quotes before and after the date format string. For example:
initialize()=
declare
          df = varchar(20) not null;
enddeclare
{
          df = '''’.''';
          set decimal :df;
}
[No]Flatten
Set flatten specifies that query flattening be used to optimize queries, including queries involving aggregate subselects or singleton subselects.
By default, Ingres performs query flattening. (The ANSI entry SQL-92 standard specifies that query flattening is not performed.)
Set noflatten disables query flattening.
[No]Hash
Set hash allows the query optimizer to consider using hash aggregation and hash joins.
Set nohash stops the query optimizer from considering the use of hash aggregation and hash joins.
Set hash is the default.
[No]Io_trace
Set io_trace prints information about disk I/O during the life of each query in the session. Using this option requires trace privilege.
Set noio_trace disables this tracing option.
See I/O Tracing in the Ingres System Administrator Guide for operating system-specific command and output examples.
Joinop [No]Timeout
This statement changes the timeout point of the query optimizer. When the query optimizer is checking query execution plans, it stops when it believes that the best plan that it has found takes less time to execute than the amount of time already spent searching for a plan or when it has evaluated all possible query plans, whichever is reached first.
If a set joinop timeout nnn is issued (where nnn is in milliseconds) the query optimizer stops looking for query execution plans after the specified number of milliseconds and uses the best plan found to that point.
If 0 is specified, the timeout occurs when the optimizer finds that the best plan found so far will take less time to execute than the amount of time already spent evaluating plans (the Ingres default).
If a set joinop notimeout statement is issued, the optimizer searches all possible query plans.
A set joinop timeout statement restores the default timeout.
This option has no effect if the greedy option is in effect.
Joinop Timeoutabort
Set joinop timeoutabort nnn specifies the time in milliseconds after which the optimizer stops considering query plans and uses the best plan found to that point. If no plan is found within the specified time, an error is generated.
If 0 (the default) is specified, the timeout is disabled.
Like set joinop timeout nnn, set joinop timeoutabort nnn instructs the optimizer to time out after nnn milliseconds of processing.
The timeoutabort option, however, also applies to the time prior to obtaining the first query plan. If the time expires and no plan is found, the query is aborted with an error. If at least one plan is found, the best plan is used and the query is executed.
Contrast this with set joinop timeout nnn, where the time can expire only after the first plan is found.
As with set joinop timeout, the default value of 0 for set joinop timeoutabort effectively disables this feature.
If both timeout and timeoutabort have been assigned non-zero values, the following rules apply:
•If timeout is greater than or equal to timeoutabort, this is equivalent to timeout being set to 0, that is, its value is ignored and the timeoutabort mechanism is in effect.
•If timeoutabort is greater than timeout, then both timing strategies are in effect.
•If the timeout time expires, the search for a better plan stops if any plan is found.
•Otherwise, optimization continues until either a plan is found or the timeoutabort timer expires.
As with set joinop timeout nnn, this new timing feature does not affect the default set joinop timeout behavior, which is to time out when as much time has been used for optimization as the estimated execution time of the best plan found so far.
Joinop [No]Greedy
This statement enables or disables the complex query enumeration heuristic of the query optimizer. The greedy heuristic enables the optimizer to produce a query plan much faster than with its default technique of exhaustive searching for query execution plans when the query references a large number of tables. For details on the greedy optimization heuristic, see the Ingres Database Administrator Guide.
When set joinop greedy is specified, the set joinop timeout statement has no effect.
[No]Journaling
The default for journaling for a DBMS Server is established by the DEFAULT_JOURNALING option in CBF. Default journaling can be overridden by the set [no]journaling statement, which then defines the default journaling state for tables created by the session after the statement is issued. If a table is created with the with [no]journaling clause that becomes the default journaling state for that table.
Important!  Regardless of whether journaling is enabled for a table, journaling only occurs when journaling is enabled for the database. Journaling for the entire database is turned on or off using the ckpdb command. For more information about ckpdb, see the Ingres Command Reference Guide.
If the current journaling status of the table is OFF, and you want to enable journaling for the table after the next checkpoint, use the set journaling on table_name statement.
Note:  Journaling status can be enabled only when table is first created or after a checkpoint in which case the checkpoint has a consistent version of the table against which subsequent journals can be applied.
To disable journaling against a table, use the set nojournaling on table_name statement.
The help table table_name statement shows the journaling status of a table. The infodb command shows the journaling status of a database. Journaling can be stopped for a database by using the -disable_journaling option of the alterdb command or ckpdb with the -i flag. (For more information, see the Ingres Command Reference Guide.)
Lockmode
The set lockmode statement changes the system defaults for locking that are in effect for a session (as defined for the DBMS in CBF). These defaults can be changed for any tables and indexes accessed during the session.
Use the set lockmode statement to optimize performance, alter the level of concurrency, or set locking back to either session or system level.
The set lockmode statement cannot be issued within a transaction, except for the statement:
set lockmode … with timeout=n|session|system|nowait
The following set lockmode parameters control locking for a session:
level
Specifies the level at which locks will be taken. It must be one of the following:
row
Takes row-level locks.
If row-level locking is specified and the number of locks granted during a query exceeds the system lock limit or the number of locks defined for the transaction, locking escalates to table level. This escalation occurs automatically and is independent of the user.
page
(Default) Takes page-level locks.
If page-level locking is specified and the number of locks granted during a query exceeds the system lock limit or the number of locks defined for the transaction, locking escalates to table level. This escalation occurs automatically and is independent of the user.
table
Takes table-level locks.
session
Takes locks according to the default in effect for the session.
system
Starts with page-level locking. If the optimizer estimates that more than maxlocks pages are referenced, escalates to table level locking.
MVCC
Uses multiversion concurrency control, which does not use page or read row locks. For more information, see the chapter on MVCC in the Ingres Database Administrator Guide.
readlock
Specifies the type of locking that applies when accessing a table to read the rows. It does not apply when accessing the table for insert, update, or delete or a table that is the object of an insert ... into select ... from or create table ... as select. Any of the following modes can be specified:
nolock
Takes no locks when reading data.
shared
Takes shared locks when reading data; this is the default mode of locking when reading data.
exclusive
Takes exclusive locks when reading data; useful in “select-for-update” processing within a multi-statement transaction.
session
Takes locks according to the current readlock default for your session.
system
Takes locks according to the readlock default, which is shared.
maxlocks
Specifies the maximum number of logical locks held by a transaction before locking escalates to a table lock. The number of locks available depends on your system configuration. The following escalation factors can be specified:
n
Specifies the number of logical locks to allow before escalating to table level locking. n must be an integer greater than 0.
session
Specifies the current MAXLOCKS default for your session.
system
Specifies the MAXLOCKS default, which is 50.
timeout
Specifies how long, in seconds, a lock request can remain pending. If the DBMS Server cannot grant the lock request within the specified time, the request aborts. Valid settings are:
n
Specifies the number of seconds to wait. n must be an integer between 0 and 2,147,483,647. If 0 is specified, the DBMS Server waits indefinitely for the lock.
nowait
Specifies that when a lock request is made that cannot be granted without incurring a wait, control is immediately returned to the application that issued the request.
session
Specifies the current timeout default for the session.
system
Specifies the default, which is no timeout.
[No]Lock_Trace
The set lock_trace statement enables the display of locking activity for the current session, including information about the locks taken and locks released. Lock tracing can be started or stopped at any time during a session. For more information about the usage and output of this statement, see the Ingres Database Administrator Guide.
Important!  Use set lock_trace as a debugging or tracing tool only. Lock_trace is not a supported feature, which means that the format and layout of the output can be changed without prior notification.
[No]Logdbevents
The set [no]logdbevents option enables or disables logging of event trace information for the application that raises events. When logging is enabled, event trace information is written to the installation log file.
To enable logging, specify set logdbevents.
To disable logging, specify set nologdbevents.
Only events raised by the application issuing the set statement are logged. Events received by the application are not logged.
[No]Logging
The set nologging statement allows a session to bypass the logging and recovery system. This may speed up certain types of update operations but must be used with extreme caution.
The set nologging statement is intended to be used for operations for which the reduction of logging overhead and log file space usage outweigh the benefits of having to recover from transaction abort. Do not use the set nologging statement to try to improve performance during everyday use of a production database.
Caution! If journaling is set for the database within a session where nologging was set, none of the changes will be journaled.
When transaction logging is disabled, any error that occurs when the database is being updated (including interrupts, deadlock, lock timeout, and forced abort) or any attempt to rollback a transaction causes the DBMS Server to mark the database inconsistent.
To use the set nologging option it is recommended that the DBA:
•Obtain exclusive access on the database to ensure that no other sessions are active against the tables being updated.
•Be prepared to recover the database. There are two cases:
–For existing databases: Checkpoint the database prior to executing the set nologging statement. If an error occurs, the database can be restored from the checkpoint.
–Loading a new database: If an error occurs, destroy the inconsistent database, create a new database, and then restart the load operation.
To enable transaction logging, issue the set logging statement.
This set statement can be issued only by the DBA of the database on which the session is operating and cannot be issued within a multi-statement transaction.
If set nologging was in effect on a journaled database, we recommend that a checkpoint be taken immediately after the set logging statement is issued. When a session in set nologging mode disconnects from a database, the DBMS Server executes a set logging operation to bring the database to a guaranteed consistent state before completing the disconnect.
Session disconnect is an asynchronous operation. The user should issue an explicit set logging statement if subsequent operations rely on logging being enabled before they are executed.
Set nologging is rejected if the database is set to always be logged.
Note:  Set nologging is compatible with read-only MVCC queries.
[No]Log_trace
Set log_trace starts tracing of logfile writes. To stop tracing of logfile writes, use the set nolog_trace statement. Using this option requires trace privilege.
When you use set log_trace during a session, you receive a list of the log records written during execution of your query, along with other information about the log, including:
•The length of the log and the amount of space reserved for its CLR
•If the log_write is a normal log record (do/redo) or a CLR
•If the log record can be copied to the journal file
•If the log is associated with a special recovery action
For information on the use of the set log_trace statement, see the Ingres Database Administrator Guide.
[No]Maxconnect
The set maxconnect statement is an alias for set maxidle. For details, see [No]Maxidle.
[No]Maxcost
The set maxcost statement restricts the maximum cost per query on the database in terms of disk I/O and CPU usage. The maxcost value must be less than or equal to the session's value for query_cost_limit. If no query_cost_limit is set, there is no limit on cost usage per query. To set query_cost_limit for a user, use the grant statement.
When maxcost is set, it remains in effect until another set maxcost or set nomaxcost statement is issued or the session terminates.
For more information, see query_cost_limit in the description of Grant (privilege) Statement.
[No]Maxcpu
The set maxcpu statement restricts the maximum CPU usage per query on the database. The maxcpu value must be less than or equal to the session's value for query_cpu_limit. If no query_cpu_limit is set, there is no limit on CPU usage per query. To set query_cpu_limit for a user, use the grant statement.
When MAXCPU is set, it remains in effect until another set maxcpu or set nomaxcpu statement is issued or the session terminates.
For more information, see query_cpu_limit in the description of Grant (privilege) Statement.
[No]Maxidle
The set maxidle option specifies whether a time limit is in force, and how long it is in seconds. The value entered must be less than that defined by the idle_time_limit session privilege. If no idle_time_limit is set, there is no time limit.
When maxidle is set, it remains in effect until another set maxidle or set nomaxidle statement is issued, or the session terminates. For more information, see idle_time_limit in the description of Grant (privilege) Statement.
[No]Maxio
The set maxio statement restricts the estimated number of I/O operations that can be used by each subsequent query to the value specified. Value must be less than or equal to the session's value for query_io_limit. If no query_io_limit is set, there is no limit on the amount of I/O performed
When MAXIO is set, it remains in effect until another set maxio or set nomaxio statement is issued or the session terminates.
To set query_io_limit for a user, use the grant statement. For more information, see Grant (privilege) Statement.
[No]Maxpage
The set maxpage statement restricts the maximum number of pages per query on the database. The value must be less than or equal to the session's value for query_page_limit. If no query_page_limit is set, there is no limit on max page usage per query.
When maxpage is set, it remains in effect until another set maxpage or set nomaxpage statement is issued or the session terminates.
To set query_page_limit for a user, use the grant statement. For more information, see Grant (privilege) Statement.
[No]Maxquery
The set maxquery statement is an alias for set maxio. For details, see [No]Maxio.
[No]Maxrow
The set maxrow statement restricts the estimated number of rows that can be returned by subsequent queries. Value must be less than or equal to the session's value for query_row_limit. If no query_row_limit is set, there is no limit on the number of rows returned.
When MAXROW is set, it remains in effect until another set maxrow or set nomaxrow statement is issued, or the session terminates.
For more information, see query_row_limit in the description of Grant (privilege) Statement.
Money_format
Set money_format specifies the format for money output. This option corresponds to the Ingres environment variable II_MONEY_FORMAT and is assigned the same values. The default is L:$. The symbol to the left of the colon indicates the location of the currency symbol. It must be "L" for a leading currency symbol or a "T" for a trailing currency symbol. The symbol to the right of the colon is the currency symbol you want displayed. Currency symbols can contain up to four physical characters and must contain leading and trailing single quotes (for example, 'T:DM').
For example:
Logical Definition
Result
L:$
$100
T:DM
100DM
T:F
100F
Money_prec
Set money_prec specifies the number of decimal places to be displayed for money values. This option corresponds to the Ingres environment variable II_MONEY_PREC and is assigned the same values. Valid values are 0, 1, and 2. The default is 2 (for decimal currency).
[No]Ojflatten
Set noojflatten tells the query optimizer not to use the transformation that converts a not exists/not in subselect to an outer join with the containing query. The default is set ojflatten.
[No]Optimizeonly
Optimizeonly specifies whether query execution should halt after the completion of query optimization.
Use set optimizeonly in conjunction with set qep when there is a requirement to view query execution plans without executing a query.
[No]Parallel
The set parallel statement controls whether the query optimizer enables the generation of parallel query execution plans.
The optional degree of parallelism value indicates the number of exchange nodes (or points of concurrency) built into the plan. The default value is 4 and the maximum is 32.
The set noparallel statement (or setting the degree of parallelism value to 0 or 1) tells the optimizer not to consider parallel query plans.
Note:  When tracing the I/O or the locks of a parallel query (using set io_trace or set lock_trace with set parallel), the trace messages from child threads of the QEP are logged to the II_DBMS_LOG. The trace messages for the main thread are sent to the user session in the normal manner.
For a discussion of parallel query plans, see the Ingres Database Administrator Guide.
[No]Printdbevents
The set [no]printdbevents option enables or disables display of event trace information for the application that raises events.
To enable the display of trace information, specify set printdbevents.
To disable the display of trace information, specify set noprintdbevents.
This option displays only events raised by the application issuing the set statement. Events received by the application are not displayed.
[No]Printqry
The set printqry statement displays each query and its parameters as it is passed to the DBMS Server for processing.
The set noprintqry option disables this option.
[No]Printrules
The set printrules statement causes the DBMS Server to send a trace message to the application each time a rule is fired. This message identifies the rule and the associated database procedure that is invoked as a result of the rule's firing.
Issue the set noprintrules to disable rule-related trace messages. By default, rule-related trace messages are not displayed.
[No]Qep
The set qep statement displays a diagrammatic representation of the query execution plan chosen for the query by the optimizer. The set noqep option disables this option.
For a discussion of query execution plans, see the Ingres Database Administrator Guide.
Random_seed
This statement sets the beginning value for the random functions. There is a global seed value and local seed values. The global value is used until you issue set random_seed, which changes the value of the local seed. Once changed, the local seed is used for the entire session. If you are using the global seed value, the seed is changed whenever a random function executes. This means that other users issuing random calls enhance the “randomness” of the returned value. The seed value can be any integer.
If you omit the value, the value is a multiple of the process ID and the number of seconds since 1/1/1970.
Result_Structure
The set result_structure statement changes the default storage structure for tables created with the create table...as select statement.
This storage structure can be any of the structures described in the MODIFY statement, that is, heap, cheap, heapsort, cheapsort, hash, chash, btree, cbtree, isam, or cisam.
The default storage structure for a table created by the create table as statement is cheap. For example:
set result_structure hash;
create temp as select id ... ;
does the same as:
create temp as select id ... ;
modify temp to hash;
For btree, cbtree, hash, chash, isam, and cisam, the newly created table is automatically indexed on the first column. This results in the above table “temp” being created with a storage structure of hash, keyed on the first column “id”.
Role
Set role allows the session role to be changed during the life of the session.
Set role has the following syntax:
set role none | role [with password = 'role_password']
If set role none is specified, the session has no role.
If set role role is specified, the role is set to the indicated role. The user must be authorized to use that role. If the role has a password, the password must be specified using the with password clause.
If the user is not authorized to use the role or the password is incorrect, the session role is unchanged.
If a role has associated subject privileges or security audit attributes, these are added to the maximum privilege set for the session when the role is activated, and removed from the privilege set when the role is deactivated. Role security audit attributes can increase auditing over the current session value but cannot decrease it.
[No]Rules
The set norules option disables any rules (user or system created) that would have been executed during the session after this statement is executed. To re-enable rules firing, issue the set rules statement. By default, rules are enabled.
Caution!  After issuing the set norules statement, the DBMS Server does not enforce check and referential constraints on tables or the check option for views.
Session With On_error
The set session with on_error option specifies how transaction errors are handled in the current session.
To direct the DBMS Server to rollback the current transaction if an error occurs, specify rollback transaction.
To direct the DBMS Server to rollback only the current statement, specify rollback statement. This is the default.
To determine the current status of transaction error handling, issue the select dbmsinfo('on_error_state') statement.
Specifying rollback transaction reduces logging overhead and can help performance. The performance gain is offset by the fact that, if an error occurs, the entire transaction is rolled back. The following errors always roll back the current transaction regardless of the current transaction error-handling setting:
•Deadlock
•Forced abort
•Lock quota exceeded
To determine if a transaction was aborted as the result of a database statement error, issue the select dbmsinfo('transaction_state') statement. If the error aborted the transaction, this statement returns 0, indicating that the application is currently not in a transaction.
The set session with on_error statement cannot be issued from within a database procedure or multi-statement transaction.
Note:  SQL syntax errors (including most messages beginning with E_US) do not cause a rollback. Only errors that occur during execution of the SQL statement cause a rollback.
Session With On_user_error
The set session with on_user_error option enables you to specify how user errors are handled in the current session. To direct the DBMS to roll back the effects of the entire transaction if a user error occurs, specify rollback transaction. To revert back to default behavior, specify norollback.
Session Authorization
The set session authorization statement allows a user with security or dbadmin to set the effective user for the current session.
The set session statement must be issued before a transaction commences and the settings apply until that session is completed.
The set session authorization options are as follows:
username
The user name specified
user | current user | session user
The current user of the session (Ingres user)
system_user
The operating system user who started the session
initial_user
The Ingres user ID that started the session
Session [No]Cache_Dynamic
Set session [no]cache_dynamic enables or disables the caching of query plans for cursors defined with dynamic select statements. It overrides the server level setting defined by the cache_dynamic configuration parameter. To check the current setting, use select dbmsinfo ('cache_dynamic').
The set session [no]cache_dynamic setting persists for the duration of the session.
See also [No]Cache_dynamic.
[No]Statistics table_name
Set nostatistics tells the query optimizer to ignore any statistics found in iihistograms or iistatistics for the specified table and to use its default values.
Set statistics tells the query optimizer to use any statistics found in iihistograms or iistatistics for the specified table.
The default is for the query optimizer to use statistics.
[No]Trace Output
Set trace output filename sets the current DBMS trace log file as filename. If a file has already been defined for tracing, Ingres uses the newly set filename for subsequent tracing. If no file is currently defined for tracing (because the II_DMBS_LOG variable is not set or set notrace output has been executed), filename will be used for subsequent tracing.
The filename option must be a quoted string containing a file name with an extension of ".log". It can be an absolute file name, or a relative file name based on the directory from which the ingstart command was used to start the DBMS Server. If filename already exists, it will be overwritten by the new tracing entries; otherwise, it will be created when the set trace output statement is executed.
Set notrace output closes the trace file. No trace information is written until a subsequent set trace output is executed to define a new trace file.
[No]Trace Point
Set trace point activates specified trace points for the current session.
Trace points are not officially supported, as Ingres reserves the right to change their effect and output without notification.
A trace point starts with a Facility Identifier followed by a number of digits. Some trace points have additional parameters. For example:
•DM420
•DM421
•QS501
•QE90
Some trace points are equivalents of set statements. For example:
Set trace point qe5 is equivalent to set norules.
Set trace point OP212 is the eqivalent of setting TABLE_AUTO_STRUCTURE to ON in CBF.
Important!  Caution! Some trace points may cause irreversible damage to Ingres. Use trace points at your own risk.
Transaction Access Mode
The set transaction access mode controls the access mode for the current transaction.
The set transaction statement must be issued before a transaction commences and the settings last only until that transaction is completed.
The access mode options are as follows:
read only
When a transaction access mode of read only is specified, insert, update, delete, copy, and DDL operations are disallowed, and an SQLSTATE of 25000 (invalid session state) is returned. Temporary tables are the exception and are always writable.
When a read only transaction is begun, it registers itself with the logging system and is allowed to proceed even when a ckpdb is pending against the database for the session.
read write
If read write is specified, the level of isolation can be READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ, or SERIALIZABLE.
Note:  The access mode of a transaction has no effect on the locking mode of the transaction.
Transaction Isolation Level
The set transaction isolation level statement controls the isolation level of the transaction.
The set transaction statement must be issued before a transaction starts and the settings last only until that transaction is completed.
The following three phenomena are relevant to isolation level:
•Dirty read—Reading a row that another session has changed but not yet committed.
•Non-repeatable read—Reading a row, then reading it again in the same transaction and getting different column values.
•Phantom read—Reading a set of rows (as specified by a where clause), then reading with the same qualification and getting a different set of rows.
Note:  All three phenomena pertain to reading. Updating a row always takes a write lock on the row and holds it until end of transaction. This discussion is for row level locks. If lockmode is set to take page or table locks, the results will be as strict or stricter.
Ingres supports the ANSI/ISO SQL92 standard levels of isolation:
read uncommitted
Allows dirty read, non-repeatable read, and phantom read.
No read locking is done.
Updaters are never blocked.
Note:  If transaction access mode is not specified and level of isolation is read uncommitted, access mode is read only; otherwise it is read write.
read committed
Prevents dirty read.
Allows non-repeatable read and phantom read.
To read a row, a readlock is taken on it and then the readlock is released after the read. Thus, if someone is changing the row, the readlock request runs into the write lock and waits.
Update transactions are only temporarily blocked.
repeatable read
Prevents dirty and non-repeatable reads.
Allows phantoms.
To read a row, a readlock is taken on it.
If the row qualifies, the readlock is held until the end of the transaction, else it is released. This blocks writers from updating a row that has been read, preventing non-repeatable read.
serializable
Locks are required on all data before being read. No locks are released until the transaction ends.
Prevents dirty read, non-repeatable read, and phantom read.
To read a row, a readlock is taken on it and the readlock is held until the end of transaction.
Depending on the storage structure, either leaf locks or value locks are taken on the rows covered by the where clause and they are held until the end of the transaction. This prevents phantoms.
Updaters cannot insert or delete qualifying rows because of the leaf or value locks.
The behaviors described above assume that the set lockmode setting for readlock has not been changed from the default, which is readlock=SHARED. The system will attempt to take out a readlock on rows as specified above, but its precise behavior is determined by the readlock setting.
The lockmode (see Lockmode) can be changed without affecting the isolation level, and vice versa. The readlock setting should be changed only with careful consideration.
[No]Unicode_substitution
When a Unicode value is being coerced into a local character set that has no equivalent character, an error is returned.
If unicode_substitution is set for the session, instead of returning the error, unmapped characters are replaced with a designated substitution character. The substitution character is the default subchar value specified in the mapping file. To override this value, provide a character value for the substitution character with this statement.
Set nounicode_substitution in the session reverts to the default behavior.
Update_Rowcount
The set update_rowcount statement specifies the nature of the value returned by the inquire_sql(rowcount) statement.
Valid options are:
changed
Returns the number of rows changed by the last query.
qualified
(Default) Returns the number of rows that qualified for change by the last query.
For example, consider the following table called test_table:
column1
column2
column3
Jones
000
green
Smith
000
green
Smith
000
green
and the following query is executed:
update test_table set column1 = 'Jones'
        where column2 = 000 and column3 = 'green';
The DBMS Server, for reasons of efficiency, does not actually update the first row because column1 already contains "Jones". However, the row does qualify for updating by the query.
If the update_rowcount option is set to changed, inquire_sql(rowcount) returns 2 (the number of rows actually changed).
If the update_rowcount option is set to qualified, inquire_sql(rowcount) returns 3 (the number of rows that qualified to be changed).
To determine the setting for the update_rowcount option, issue the following statement:
select dbmsinfo('update_rowcnt')
Examples--Set Statement
The following are set statement examples:
1.Create tables "withlog1", "withlog2", and "withlog3" with journal logging enabled and "nolog" without.
set journaling;
create table withlog1 ( ... );
create table withlog2 ( ... );
set nojournaling;
create table withlog3 ( ... ) with journaling;
create table nolog1 ( ... );
2.Create tables "a", "b", and "d" with different structures.
create table a as ...;/* HEAP - the default */
set result_structure hash hash;
create table b as select id ...;/* HASH on 'id' */
set result_structure heap;
create table d as select id ...;/* HEAP again */
3.Set lockmode parameters for the current session. Tables accessed after executing this statement are governed by these locking behavior characteristics.
set lockmode session where level = page,
    readlock = nolock,
    maxlocks = 50, timeout = 10;
4.Set the lockmode parameters explicitly for table employee.
set lockmode on employee
    where level = table, readlock = exclusive,
    maxlocks = session, timeout = 0;
5.Reset your session default locking characteristics to the system defaults.
set lockmode session where level = system,
    readlock = system,
    maxlocks = system, timeout = system;
