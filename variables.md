# Variables in OpenROAD

OpenROAD variables contain or point to data that the application manipulates. This can be data displayed to the user or used solely in scripts and procedures.

In OpenROAD, a variable is associated either with the application (global variables) or with a specific frame, procedure, method, field script, or event block (local variables).

## Global Variables

Global variables provide data pertinent to the entire application and are available for use in any script or procedure. You use the OpenROAD Workbench to declare a global variable for your application. For more information about declaring global variables, see the *Workbench User Guide*.

## Local Variables

Local variables contain data associated with a specific frame, procedure, method, field script, or event block. Local variables include:

- Variables declared in the initialize statement for a frame or a field script
  - These frame variables are not associated with fields and the information in them is not displayed to the user directly.
- Variables associated with field and menu items
  - You declare these local variables implicitly when you create the fields on a form. The information in them is displayed directly on the form, or you can declare them at runtime as described in the *Language Reference Guide* online help.
  - When a variable is associated with a field on the displayed form, setting its value updates the display to show the new value. Conversely, referring to the variable reflects the current setting of the displayed form, including updates performed by the user.
- Variables declared in a procedure, method, or event block definition
  - These variables are not associated with fields, and the information in them is not displayed to the user directly. They are available for use only in the procedure, method, or event block that defines them.

## Types of Variables

There are three types of variables in OpenROAD:

1. Simple variables
2. Reference variables
3. Dynamic array variables
