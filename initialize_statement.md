# Initialize Statement

This statement declares local variables and local procedures and performs any specified start-up operations for a frame, field, or userclass script. This statement also declares local procedures for a user class script.

This statement has the following syntax:
```
initialize [([parameterlist])] =
[declareblockwithprocedures]
beginendblock[;]
```

You can use the initialize statement to:

- Declare local variables for a frame or user class
- Execute any start-up operations that you specify for a frame or user class

The `parameterlist` can be specified only in an initialize statement for a frame script, not in a script of a field or user class. Parameters are local variables that can be specified as parameters on a callframe, openframe, or gotoframe statement. For more information about passing parameters, see:

- Callframe Statement
- Openframe Statement
- Gotoframe Statement

The declare block can include variables or procedures. For more information, see DeclareBlockWithProcedures.

All field and menu item variables that you named in the visual development environment when you created the frame are automatically declared and cannot be redeclared in the initialize section.

Also, you cannot use the initialize statement in a procedure. Local variables for a 4GL procedure are declared by the procedure statement. See the procedure statement (see Procedure Statement) for information about declaring local variables in 4GL procedures.

The initialize statement is optional, but when you use it, it must be the first statement in the frame, field, or user class script.

OpenROAD executes the initialize statements in a user class script or a frame script and its field scripts before initializing and displaying a frame or using a user class. Thus you can use the initialize statement in a frame, field, or user class script to execute any start-up operations that you want to perform before the frame is displayed or before the user class methods or attributes are accessed. However, because the frame's form is initialized after the initialize statement completes, some actions, such as setting the HasDataChanged attribute, have no effect if performed by the initialize statement.

The initialize statement in a frame script is executed before the initialize statements in field scripts for the frame. Similarly, the initialize statement in a composite field's script is executed before the initialize statements in field scripts for children of the composite field.

The `begin` and `end` keywords define the initialize statement block that contains the statements defining the start-up tasks for that frame.

Declare local variables and forward references to local procedures in the declare section of the initialize block (see DeclareBlockWithProcedures). Local procedures declared in the declare section must have a corresponding procedure statement in the same script. If a declare block is not followed by a begin block of initialization statements, `enddeclare` is required at the end of the local variable declarations in the declare block.

Notes:

- If you define global variables that map to user classes whose creation triggers constructors directly or indirectly, these will trigger before your starting component is launched, and the behavior of your application will be undefined and cannot be supported. If you need a global variable that references such a user class, declare the global variable default null, and assign the relevant object to it during application startup. In general, the execution sequence of constructors in user class/datatype variables declared in the same scope is undefined. You must ensure that there are no dependencies between such constructors.
- The posting of userevents (using SendUserEvent) within the initialize block of a user class is unsupported.

## Parameters--Initialize Statement

This statement has the following parameters:

- `parameterlist`: See NamedParameterListDefinition.
- `declareblockwithprocedures`: See DeclareBlockWithProcedures.
- `beginendblock`: See BeginEndBlock.

## Examples--Initialize Statement

Set up two simple variables that can be specified as parameters and give them default values. Also set up two reference variables, one of type ADDR and the other a dynamic array of class EMP. Create a placeholder row in the array with a Social Security number (ssn) attribute of 999999999:

```
initialize(
     idnum = integer default 0,
     idname = varchar(10) default 'none')=
declare
     addr = ADDR;
     emptable = array of EMP;
enddeclare
begin
     emptable[1].ssn = 999999999;
end
```
Declare a simple integer variable, i, and set up two reference variables, one of type EMP and the other a dynamic array of class EMP. Fill the array emparray from the emptable table:
```
initialize(
     emparray = array of EMP,
     empptr = EMP) =
declare
     i = integer;
enddeclare
begin
     i = 1;
     select :emparray[i].name = name
          from emptable
     begin
          i = i + 1;
     end
end;
```
Declare four reference variables; the first one is a collection of class worksheet that is a Microsoft Excel object. The others are references to other objects supported by Excel. The setup procedure opens a workbook and gets the worksheet named "sales" from the workbook's collection of worksheets.
```
initialize()=
declare
     xlapp           = Application;
     wrkbook         = Workbook default null;
     wrksheet        = Worksheet default null;
     sheets_coll     = Sheets collection of Worksheet default null;
     setup           = Procedure;
enddeclare
begin
end
on click setup_btn =
begin
     callproc Setup();
end
procedure Setup() =
begin
     wrkbook = xlapp.workbooks().open('example.xls');
     sheets_coll = wrkbook.worksheets();
     wrksheet = sheets_coll['sales'];
end
```
## User Class Constructors

User class constructors (initializers) extend the capabilities of OpenROAD user classes by enabling developers to specify how the user classes are initialized (what 4GL code is to be run) when they are instantiated. The capabilities of user class constructors parallel the capabilities of the initialize statement for a frame or field script.

You may specify the initialize block, the same block of code available for 4GL frames and procedures, to serve as user class constructors. The primary purpose of constructors is to execute the 4GL code needed to initialize data within the user class instance when the user class is created.

In the initialize section for a user class, you may declare variables that are local to the user class instance being created. These variables are available to all the methods defined for the user class, but hidden from the 4GL that creates the user class instance or calls the user class's methods.

When a user class instance containing a constructor is created at runtime (when a field or declared variable whose data type in that user class is instantiated, or when the 4GL code executes a create method call), the constructor code block runs to completion before the next 4GL statement is run. This means that while using the debugger:

- You could step over one statement to the next and perhaps be unaware that one or more user class constructor code blocks were run.
- You could step from one statement into the next, into a constructor block that needs to execute before the next logical statement is run in the application.

Global variables that create instances of user classes containing an initialize execute the constructors independently of the other frames, procedures, and so on, that represent an application.

Example of a user class constructor:

```
initialize =
declare
    timetwo = procedure returning integer;
    value = integer not null;
enddeclare
begin
    Curobject.uc1_min = 1;
    curobject.uc1_max = 100;
    curobject.uc1_value = ifnull(timetwo(num = 5), 0);
    value = 50;
end
method GetInstValue() =
{
    return value;
}
procedure timetwo(num = integer not null) =
{
    return num*2;
}
```
