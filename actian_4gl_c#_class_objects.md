# Actian 4GL in OpenROAD: XML to C# Class Representation

Actian 4GL in OpenROAD uses XML to represent class objects which can be directly converted to C#. Below is an example 
of the C# representation with an explanation of what each aspect represents:

```csharp
class comp : superClass {
    int i_no;
    char i_f_no;
    string v_f_name;
    int i_st;
}
```

## Class Declaration
- `class comp` declares a new class named comp.
- This class is used to encapsulate data and behavior related to a specific entity within a software system.

## Inheritance
- `: superClass` specifies that comp inherits from (extends) another class named superClass.
- Inheritance allows comp to inherit properties and behaviors from superClass, promoting code reuse and establishing 
an "is-a" relationship between comp and superClass.

## Field Declarations
- `int i_no;`, `char i_f_no;`, `string v_f_name;`, `int i_st;` declare four private fields within the comp class.
- Fields are variables that store data associated with an object of the class. They define the state or 
characteristics of instances of the class.

## Field Types
- `i_no` is an integer (`int`) field.
- `i_f_no` is a character (`char`) field.
- `v_f_name` is a string (`string`) field.
- `i_st` is another integer (`int`) field.
These fields represent specific attributes or properties (`i_no`, `i_f_no`, `v_f_name`, `i_st`) that instances of 
comp will possess.
