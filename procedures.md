# Procedures

A *procedure* is a named piece of code that performs a single task. Unlike the event-driven code that comprises frame and field script code, procedures process in sequence. Procedures also differ from frames in that they are not associated with a form. They are used primarily to eliminate duplication among your scripts and to make code more readable. Procedures also provide the means of accessing a 3GL language from an OpenROAD script.

You can define procedures in two ways:

## Globally

Procedures that are defined globally to an application can be called from any OpenROAD script in that application or, if included in another application, from a second application as well.

## Locally

Procedures that are defined locally within a frame, field, or procedure script can be called only from the script in which they are defined or from descendent fields (fields within the scope of the calling script).

OpenROAD lets you create three types of procedures:

### 4GL procedures

Global 4GL procedures are created in the OpenROAD Workbench. Local 4GL procedures are declared and defined in the frame, field, or procedure script in which they are called. For the procedure statement syntax to write a 4GL procedure, see the *Language Reference Guide* online help.

### Database procedures

Database procedures are created and maintained independently of OpenROAD, but must be registered with specific applications. They are available outside of OpenROAD applications.

### 3GL procedures

3GL procedures are created and maintained independently of OpenROAD, but must be registered with specific applications.
