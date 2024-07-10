## Type

Specifies the data type, length, nullability, and default value of a local variable or parameter.

This element has the following syntax:

*simple_datatype* [**with null** | **not null**] [**not default** | **with default** | [**with** ]**default** | *defaultvalue*] |
    [**array of**] *classname* [[**with** ]**default** [**null**]] |
    *collectionname* **collection of** *externalclassname* [[**with** ]**default** [**null**]]

### *simple_datatype*

Specifies the simple data type

### *defaultvalue*

Specifies the default value, which must be a null, a literal, or a system constant of the appropriate type. Valid system constants are listed in System Constants.

**Note:** The clauses "with null, not default" and "with default" are provided for syntactic compatibility with SQL, but they have no effect in OpenROAD.

### *classname*

Specifies the class name. It can be the name of a system class, user class, or external class.

### *collectionname*

Specifies the collection name

### *externalclassname*

Specifies the external class name
