## Call Statement

This statement calls another application.

This statement has the following syntax:

call subsystem ([parametername = value
     {, parametername = value}]);

The call statement specifies optional parameters that call another application. This call blocks all frames in the OpenROAD application until the user exits from the called subsystem. When the user exits from the subsystem, the application resumes execution with the statement following the call statement. The application can inquire about the status of the executed command by using the CurSession.CallSystemStatus attribute.

If you issue the call statement in a transaction, the statement commits the transaction without a warning message before invoking the subsystem. To ensure data integrity, if a transaction is open, terminate it before issuing the call statement.

For more information about transaction management, see How You Can Manage Transactions in the Programming Guide.

If your application is not connected to a database, the only variant of this statement that you can use is call runimage. For more information about the call statement variants, see Subsystem Types.

### Parameters--Call Statement

This statement has the following parameters:

#### subsystem

Specifies the dynamic name of a specific tool or application. For a list of valid subsystems, see Subsystem Types.

#### parametername

Specifies the dynamic name of a parameter to be passed to the subsystem. This parameter is often a synonym for a command line flag or an object name.

The parameter can be any legal simple data type (integer, float, money, date, or varchar). Dates are stored as a varchar(25) and money is stored as the float data type.

#### value

Specifies the value to be assigned to the parameter. A value can be any simple variable expression that is understandable in the context of the parameter. Its data type must be compatible with the data type of the parameter to which it is assigned. If the value is a string literal, it must appear in single quotes.

To pass a parameter without an assigned value, use an empty string.

The following sections describe the parameters and subsystem types that you can use with the call statement.

### Flag Parameters

The parameters and values that you can use in a call subsystem statement are specific to each subsystem or application. Many of these parameters are equivalent to the flags that you use when you call the subsystem directly from the operating system command line.

You can pass flags to the subsystem in two ways:

- Use parametername as a keyword synonym for a flag.
  To specify the flag, use the format:
  parametername = value

- Use flags = 'flag_list'
  where flag_list is a list of flags enclosed in single quotes. Separate individual flags in the list with blanks or tabs. Flag designations in this format are identical to those specified in the command line version of the subsystem call.

Both methods of passing flags are shown in the following example where tblfld is the 4GL synonym for the -t flag. You can call Query-By-Forms (QBF) with the -t flag using either of the following statements:

call qbf(tblfld = 'customer');
or
call qbf(flags = '-tcustomer');

You can use both of these formats in the same statement. However, do not pass the same flag twice.

For most subsystem calls, you can use the flags parameter to pass the value of any flag, including those that do not have defined 4GL keyword synonyms (parameternames). However, you cannot pass the -u flag to a called subsystem. (You can specify this flag only when you initially connect to a database at the start of the underlying application.)

The flags parameter does not exist for calls to the application subsystem. Also, calls to the Terminal Monitor (call sql) can use the flags parameter only for a few limited flags (for more information, see the call sql statement in Subsystem Types).

### Subsystem Types

The following table lists variants of the call statement. The term standard output window refers to the window from which you started OpenROAD.

- call application: Calls an Applications-By-Forms (ABF) application and executes the application in the standard output window. For more information, see Call Application Statement.
- call isql: Calls Interactive SQL and executes it in the standard output window
- call qbf: Calls Query-By-Forms (QBF) and executes it in the standard output window
- call report: Calls Report-Writer to run a report and executes it in the standard output window
- call rundbapp: Calls an application stored in the database associated with the calling application's current database session
- call rungraph: Calls the Visual-Graphics-Editor to display a graph defined by VIGRAPH and executes it in the standard output window
- call runimage: Calls another 4GL application from a runimage file
- call sql: Calls the SQL Terminal Monitor and executes it in the standard output window

### Call Application Statement

This statement calls the specified ABF or Vision Pro application:

This statement has the following parameters:

#### name

Specifies the ABF or Vision Pro application name.
If you specify the name parameter, you cannot use the executable parameter.
If this is not the name of a file in the current directory, you must specify a full path name.

#### frame

Specifies the first frame or procedure to be called in the ABF or Vision Pro application. The value is a frame or procedure name.
If the called application was built with a default first frame or procedure, this parameter is optional. If you do not include it, the application begins at the default frame or procedure.
If the called application has no default first frame or procedure or you do not want to start at the default, you must include this parameter.

#### executable

Specifies the executable file containing the ABF or Vision Pro application.
If you specify the executable parameter, you cannot use the name parameter.
If this is not the name of an application in the current directory, you must specify a full path name.

#### param

Passes application-specific parameters to the called application. The value is a string of up to 2000 bytes that contains the parameters you want to pass. Use single spaces to separate the parameters.

OpenROAD passes the parameters to the application as a series of parameters to the command that starts the application. Your host system that processes this command may modify these parameters before the application receives them.

To call another OpenROAD application, use the call runimage or call rundbapp statement. To start a second application that uses a different database or database session, use the call system statement.

### Call ISQL

Calls Interactive SQL. It has no parameters.

### Call QBF

Calls Query-By-Forms (QBF) to use with the specified QBFName, JoinDef, or table. It takes the following parameters:

#### qbfname

Specifies a QBFName. If the value is blank, the statement starts QBF at the QBFNames Catalog frame.
Equivalent to the -f flag.

#### joindef

Specifies the name of a JoinDef. If the is blank, the statement starts QBF at the QBF JoinDefs Catalog frame.
Equivalent to the -j flag.

#### tblfld

Specifies a table name. This parameter runs QBF on the specified table, using a table field to display the data. If the value is blank, the statement starts QBF at the Tables Catalog frame.
Equivalent to the -t flag.

#### lookup

Specifies a QBFName, JoinDef name, or a table name, which are checked in that order.
Equivalent to the -l flag.

#### silent

Suppresses status messages. The value is ignored.
Equivalent to the -s flag.

#### mode

Lets the application user enter Query-by-Forms directly in the specified mode. The value can be retrieve, append, update, or all.
Equivalent to the -m flag.

#### table

Specifies the name of the table to query with QBF. The value is a table name. This parameter must be omitted if either the joindef, qbfname, tblfld, or lookup parameter has been used.

#### flags

Specifies the flags that are to be in effect. The value consists of a list of flags enclosed in single quotes, with items separated by blanks or tabs. All of the flags for the system-level command line (except -u and -G) are allowed.

### Call Report

This statement calls Report-Writer to run the specified report. It takes the following parameters:

#### file

Specifies a file name. This parameter directs the formatted report to the specified file for output.
Equivalent to the -f flag.

#### silent

Suppresses status messages. The value is ignored.
Equivalent to the -s flag.

#### report

Specifies a report name. This parameter indicates that a report, rather than a table, is being specified.
Equivalent to the -r flag.

#### mode

Specifies that a table, rather than a report, is being specified. (The name of the table is the value for the name parameter.) The value can be column, wrap, or block.
Equivalent to the -m flag.

#### mxline

Sets the maximum output line size to the specified number of characters. The value is an integer.
Equivalent to the -l flag.

#### mxquer

Sets the maximum length of the query specified in the .query command, after all substitutions for runtime parameters have been made, to the specified number of characters. The value is an integer.
Equivalent to the -q flag.

#### mxwrap

Sets the specified number as the maximum number of lines to wrap with one of the column C formats or the maximum number of lines that can be used within any block. The value is an integer.
Equivalent to the -w flag.

#### name

Names a database table or view for which a default report is to be formatted. The value is a table or view name.

#### param

Passes the parameters for the report. The value is a quoted string containing the parameter string for the report. Each element in a param string must have this form:
pname = 'value'
The elements must be separated by blanks or by tabs. Non-numeric values being passed in an element must be enclosed in standard quotes.
There is one exception: If pname is a character report parameter, the value must be enclosed in double quotes within single quotes (' " " '). Embedded double quotes must be dereferenced with a backslash (\).

#### printer

Specifies a printer name (overrides the default printer).
Equivalent to the -o flag.

#### copies

Specifies the number of copies to be printed. The value must be a positive integer. This parameter can only be used if you have specified the printer parameter also.
Equivalent to the -n flag.

#### forcerep

Specifies that the Report-Writer creates headers and footers, even if there is no data for the report. There is no value.
Equivalent to the -h flag.

#### formfeed

Specifies that the Report-Writer forces form feeds at page breaks, overriding any settings in the report-formatting commands. The value is ignored.
Equivalent to the +b flag.

#### noformfeed

Specifies that the Report-Writer suppresses form feeds, overriding any settings in the report-formatting commands. The value is ignored.
Equivalent to the -b flag.

#### pagelength

Sets the page length, in lines, for the report, overriding any .PL commands in the report. The value is an integer.
Equivalent to the -v flag.

#### sourcefile

Requests that the report specification be read from a source file outside of the Reports Catalog. With this flag, the specified source file is not saved in the database. The value is a file name.
Equivalent to the -i flag.
Note: You cannot use this flag in conjunction with the report (-r) or mode (-m) parameters.

#### brkfmt

Specifies that breaks and calculations for dates and numbers are based on the displayed data rather than on the internal database values. The value is ignored.
Equivalent to the +t flag (default).

#### nobrkfmt

Specifies that breaks and calculations for dates and numbers are based on the internal database values rather than on the displayed data. The value is ignored.
Equivalent to the -t flag.

#### flags

Specifies the flags that are to be in effect. The value consists of a list of flags enclosed in single quotes with items separated by blanks or tabs. All of the flags for the system-level command line (except -u and -G) are allowed.

### Call Rundbapp

This statement starts execution of another OpenROAD application. The OpenROAD application that is called must be stored in the database associated with the current session of the calling application. Frames from the calling application are inactive while the called application is executing. This statement takes the following parameters:

#### application

Specifies the application to be run. The value is the name of the application. (The called application must be stored in the database associated with the current session of the calling application.)

#### startcomponent

Specifies the frame or procedure in the called application that you want as the entry to the called application. The value is the name of the starting component. If you do not specify a starting component, OpenROAD uses the default starting frame specified in the property sheet for the application.

#### loadmodule

Specifies the directory location of the 3GL load module for the application. For more information about 3GL load modules, see the Programming Guide. If you do not specify this parameter, no 3GL procedures can be called from the called application.

Note: If you want to execute an application stored in another database, use the call system statement.

### Call Rungraph

This statement calls the Visual-Graphics-Editor (VIGRAPH) to display the specified graph that was defined by VIGRAPH. This statement takes the following parameters:

#### graph

Runs VIGRAPH to display the named graph. The value is the graph name.

#### device

Causes VIGRAPH to plot the graph in the correct format for the specified device type. The value is the device name.
Equivalent to the -d flag.

#### file

Causes VIGRAPH to send the graph output to the specified file. The value is a file name.
Equivalent to the -f flag.

#### table

Causes VIGRAPH to plot the graph with data from the named table. The value is a table name.
Equivalent to the -t flag.

#### plevel

Causes VIGRAPH to plot the graph at the specified presentation level. The value is an integer from one to five.
Equivalent to the -p flag.

#### prompt

Causes VIGRAPH to display a message that the graph has been plotted. There is no value.
Equivalent to the -n flag.

#### flags

Specifies the flags that are to be in effect. The value consists of a list of flags enclosed in single quotes with items separated by blanks or tabs. All the flags for the system-level command line are allowed.

### Call Runimage

This statement starts execution of another OpenROAD application stored in an image file. The called application runs in the same database connection as the calling application. While the called application is executing, no frames in the calling application are active. This statement takes the following parameters:

#### filename

Specifies the name of the image file that contains the application to be run. The value is the file name.

#### startcomponent

Specifies the name of the starting frame or procedure for the called application. The value is the name of the starting component. If not specified, the default starting frame specified in the property sheet for the application is used.

#### loadmodule

Specifies the directory location of the 3GL load module for the application. For more information about 3GL load modules, see the Programming Guide. If you do not specify this parameter, no 3GL procedures can be called from the called application.

Note: To execute an application that uses another database, use the call system statement.

### Call SQL

This statement calls the SQL Terminal Monitor. This statement takes the following parameter:

#### flags

Specifies the flags that are to be used. The value consists of a list of flags enclosed in single quotes with items separated by blanks or tabs. You can use the following flags: +a|-a, +d|-d, +s|-s.

### Examples--Call Statement

Call the OpenROAD inventory application stored in the same database, beginning at the parts frame:

call rundbapp(application = 'inventory',
          startcomponent = 'parts');

Call the OpenROAD application specified in the runimage variable of the current frame, starting at the frame in the framename field:

commit;
call runimage(filename = 'runimage',
          startcomponent = 'framename');

Call QBF with no parameters. The QBF catalog frame is displayed:

commit;
call qbf();

Run Query-by-Forms in insert mode, suppressing status messages, using the QBFName expenses:

commit;
call qbf(qbfname = 'expenses',
          flags = '-mappend -s');

Run a default report on emptable in the column mode:

commit;
call report(name = 'emptable', mode = 'column');

Run a report on a temporary table. Both the temporary table's name and contents are based on user input:

create table :tbl as select * from orders
          where status = :status and custno = :custno;
commit;
call report(name = 'orders',
          param = 'table = "' + :tbl + '"');
