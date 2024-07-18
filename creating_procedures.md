# How You Can Create Procedures in OpenROAD

## Creating a global procedure

Creating a global procedure in OpenROAD is a two-step process:

1. Declare the procedure in OpenROAD with the Procedure Editor. At this stage, you give the procedure a name and declare the return value type. For instructions about this process, see the Workbench User Guide.

2. Write the body of the procedure directly in the Script Editor using the procedure statement.

## Creating a local 4GL procedure

Creating a local 4GL procedure in OpenROAD is also a two-step process:

1. Declare the procedure directly in the frame, field, or procedure script.

2. Write the body of the procedure using the procedure statement.

## Creating database or 3GL procedures

You register and create database procedures or 3GL procedures using the appropriate Procedure Editor in OpenROAD Workbench. You write 3GL procedures as you would any 3GL program. After you create and compile the source code for a 3GL procedure, you create a dynamic-link library to link the procedure into the application.

## Calling procedures

To call a procedure, you can use the callproc statement. There are slightly different versions of the callproc statement for each type of procedure. Calling 4GL and database procedures is described in subsections of this chapter.

For more information about creating, calling, and linking 3GL procedures, see Using 3GL in Your Application.
