## With Clause

The WITH clause is used to override the called frame's original definition (found in the attributes of its GhostExec or FrameExec object). It specifies a list of attributes in the GhostExec or FrameExec definition.

Any attributes that you do not set default to the values they have in the GhostExec or FrameExec object. Any attributes that you set are valid only for that instance of the frame.

This element has the following syntax:

**WITH** *attribute* **=** *expression*{**,** *attribute* **=** *expression*}

### *attribute*

Specifies any of the attributes that you can set for the GhostExec or FrameExec class, depending on the type of frame being called

### *expression*

Specifies any legal expression that evaluates to the appropriate data type
