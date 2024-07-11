## Case Statement

This statement selects one of a number of possible actions based on the value of an expression. The expression can be any valid OpenROAD data type.

This statement has the following syntax:

```
case key of
  valuelist: statement | beginendblock
  {valuelist: statement | beginendblock}
endcase;
```
A related statement is the nextcase statement (see Nextcase Statement), which is used to jump from the current statement to the beginning of the next statement block (this is the equivalent to the fall-through case in C). See the example.

In SQL statements '' and ' ' are evaluated equally, but in a 4GL case statement they are not.

### Parameters--Case Statement

This statement has the following parameters:

#### key

Specifies a constant expression or a variable reference

#### valuelist

Specifies a single expression or a list of expressions of the same type as the `key` expression, separated by commas, or the keyword **default**

#### statement

Specifies a single statement

#### beginendblock

See BeginEndBlock.

### Examples--Case Statement
```
case inputselection of
          'Beginner',
          begin_synonym :
          {
               field(menu.edit_menu.paste).curbias = MB_INVISIBLE;
               field(menu.file_menu.save_spec).curbias = MB_INVISIBLE;
               curframe.WindowTitle = curframe.WindowTitle + 'Beginner';
          }
          'Intermediate',
          inter_synonym :
          {
               field(menu.edit_menu.paste).curbias = MB_INVISIBLE;
               /*
               ** Do advanced stuff also.
               */
               nextcase;
          }
          'Advanced',
          advance_synonym :
          {
               field(menu.file_menu.save_spec).curbias = MB_ENABLED;
          }
          default:
               message 'Unknown selection' + inputselection + '!';
          endcase;
```
Note several things about this example:

- The value of the key expression and the valuelist elements need not be integers (in fact they can be any type of expression including floating point or string values), and they need not be constants (they can be simple variable references also).
- To "fall through" from one case block to the next, you must explicitly say so in the code, or the code may have unintentional effects.
- There can be only one default statement block in any case statement (it does not have to be at the end), and there does not have to be a default block.
- If only a single statement is necessary to execute the code for a case, you do not have to use begin and end to bracket it, just code the statement. This can lead to briefer code:

```
case lowercase(inputselection) of
     default : value = 0;
     'one'   : value = 1;
     'two'   : value = 2;
     'three' : value = 3;
endcase;
```
This type of construct is the equivalent of an array lookup using a string value as the index.

One of the most used multi-way branches in OpenROAD is to choose among a set of options based on the class of an object. This is coded as follows:

```
if obj.IsA(class = SubForm) = TRUE then
          ...
elseif obj.IsA(class = CompositeField) = TRUE then
          ...
elseif ...
```
By using the attribute of an object called Class that returns a pointer to the class object of the object, and by using the case statement construct, this can be coded cleaner and executed faster as:
```
case obj.Class of
          SubForm : ...
          CompositeField : ...
          ...
endcase;
```
