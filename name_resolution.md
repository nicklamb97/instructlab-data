# Name Resolution

OpenROAD application components share the same name space. This means that OpenROAD identifies the component strictly by name and does not consider the context in which the name is used. These components are:

- Variables
- Named constants
- Frames
- Procedures
- Classes

When OpenROAD encounters a name, it uses the first component it finds that matches the name. The context in which the name is specified has no effect on the name resolution. For example, when you use a procedure name in a callproc statement, OpenROAD searches for any component with that name, not just a procedure.

OpenROAD searches for names in the following order:

1. Current event block, local procedure, or user class method
2. Current field script
3. Enclosing field scripts (which are field scripts for fields containing the current field script's field)
4. Current frame, global procedure, or user class script
5. Current application
6. Included applications (in the same order they were included)
7. Core library

If the component belongs to a frame, procedure, method, or user class script, you cannot change the search order. However, if the component belongs to an application, you can direct OpenROAD to search only within a specific application by qualifying the component name with the application's name. The syntax is:

When you include the application name, OpenROAD searches the specified application for the component rather than using the usual search order. You can specify the current application, any included application, or the core library.

When you specify a procedure or frame name at runtime, the search order differs from the first four steps of the name order search.

## Dynamic Frame and Procedure Name Resolution

The following four statements let you specify a frame or procedure name at runtime:

- Callframe
- Openframe
- Gotoframe
- Callproc

When you use one of these statements to specify a frame or procedure name, OpenROAD resolves the reference at runtime instead of at compile time. OpenROAD first searches the currently executing component (a frame, global procedure, or user class method script) for a local procedure with the dynamically specified name. If the search is not successful, OpenROAD then searches for a global component (a frame or procedure).

By default, the global search order for the resolution starts with the topmost running application, that is, the application with which the user began the session.

This feature means that a frame in an included application can call a frame that belongs to the including application. For example, if Application A includes Application B, then Application B can use a dynamic name to call a frame that is part of Application A.

The following code example uses three applications:

**Application A**
Contains frames A1, A2, and X. The application also includes Application B and C. It is the starting application for this example.

**Application B**
Contains frames B1, B2, and X. The application also includes Application C.

**Application C**
Contains frames C1, C2, and X.

Assume that each of the frames in each of these applications has the following variable assignments:

/* The variables are varchar variables */
nm1 = 'A1';
nm2 = 'B1';
mn3 = 'C1';
nm4 = 'X';
nm5 = 'A!X';
nm6 = 'C!X';

The following table displays dynamic and explicit name resolution. It also shows the behavior for a variety of callframe statements using the assumptions described in this section. It also illustrates the behavior differences between dynamic and explicit name resolution.

| Statement | From AppA | From AppB | From AppC |
|-----------|-----------|-----------|-----------|
| callframe A1 | calls A!A1 | compile warning and runtime error | compile warning and runtime error |
| callframe :nm1 | calls A!A1 | calls A!A1 | calls A!A1 |
| callframe B1 | calls B!B1 | calls B!B1 | compile error |
| callframe :nm2 | calls B!B1 | calls B!B1 | calls B!B1 |
| callframe C1 | calls C!C1 | calls C!C1 | calls C!C1 |
| callframe :nm3 | calls C!C1 | calls C!C1 | calls C!C1 |
| callframe X | calls A!X | calls B!X | calls C!X |
| callframe :nm4 | calls A!X | calls A!X | calls A!X |
| callframe A!X | calls A!X | compile error | compile error |
| callframe :nm5 | calls A!X | calls A!X | calls A!X |
| callframe C!X | calls C!X | calls C!X | calls C!X |
| callframe :nm6 | calls C!X | calls C!X | calls C!X |
