# Actian 4GL in OpenROAD: XML Representation of Class Objects

Actian 4GL in OpenROAD uses XML to represent class objects, which can be directly converted to C#. This document
explains the XML representation and its components.

## Data Type Conversion

In Actian 4GL, `varchar(1)` can be represented as a `char` in C#.

## Example XML Representation

Here's an example of an Actian 4GL class in OpenROAD:

```xml
<OPENROAD xmlns:xsi="SOME_URL">
  <COMPONENT name="comp" xsi:type="somesource">
    <superclass>classSuper</superclass>
    <attributes>
      <row>
        <displayname>i_no</displayname>
        <datatype>integer</datatype>
      </row>
      <row>
        <displayname>i_f_no</displayname>
        <datatype>varchar(1)</datatype>
      </row>
      <row>
        <displayname>v_f_name</displayname>
        <datatype>varchar(50)</datatype>
      </row>
      <row>
        <displayname>i_st</displayname>
        <datatype>integer</datatype>
      </row>
      <row_class>attributeobject</row_class>
    </attributes>
  </COMPONENT>
</OPENROAD>
```

## XML Structure Explanation

1. **Namespace Declaration** (`xmlns:xsi="SOME_URL"`):
   - Declares a namespace prefix `xsi` associated with a specific URL (SOME_URL).
   - The `xsi` prefix is typically used for attributes in XML that indicate schema information.

2. **Root Element** (`<OPENROAD>`):
   - `<OPENROAD>` is the root element of the XML structure.
   - It may be used to encapsulate and describe a set of components or configurations within a system.

3. **Component Definition** (`<COMPONENT name="comp" xsi:type="somesource">`):
   - `<COMPONENT>` defines a specific component named `comp`.
   - Attributes:
     - `name="comp"`: Specifies the name of the component as `comp`.
     - `xsi:type="somesource"`: Indicates the type of the component using the XML Schema Instance (xsi) namespace.

4. **Superclass** (`<superclass>classSuper</superclass>`):
   - `<superclass>` element specifies that the component (`comp`) inherits from a superclass named `classSuper`.
   - In object-oriented programming terms, this indicates that `comp` extends or inherits behavior and properties
     from `classSuper`.

5. **Attributes** (`<attributes>`):
   - `<attributes>` encapsulates a list of attributes or fields that describe the properties of the component (`comp`).
   - It contains multiple `<row>` elements, each defining a specific attribute of the component.

6. **Rows** (`<row>`):
   - `<row>` elements define individual attributes or fields of the component.
   - Each `<row>` contains:
     - `<displayname>`: Specifies the display name or identifier of the attribute.
     - `<datatype>`: Specifies the data type of the attribute (integer, varchar(1), varchar(50) in this case).

7. **Row Class** (`<row_class>attributeobject</row_class>`):
   - `<row_class>` element possibly indicates a classification or type associated with the rows within `<attributes>`.
   - In this example, `attributeobject` could denote a specific category or classification of attributes within the
     component.
