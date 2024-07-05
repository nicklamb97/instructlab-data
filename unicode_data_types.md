# Unicode Data Types

Unicode data types nchar and nvarchar are used to store Unicode data in the UTF-16 form. They behave similarly to 
char and varchar. In the case of char and varchar, the strings are encoded as single-byte or multi-byte forms based 
on the value of the II_CHARSETxx variable. If II_CHARSETxx is set to the value UTF8, the char and varchar strings 
are encoded as UTF-8 strings. UTF-8 encoded strings and UTF-16 encoded strings represent Unicode data.

The Unicode UTF-16 form encodes characters of fixed length and typically uses 16 bits. The Unicode UTF-8 form encodes 
characters of variable width between one and three bytes per character. Single-byte-encoded strings use one byte per 
character. Multi-byte-encoded strings use between one and three bytes per character.

The maximum Unicode UTF8-16 string is 16,000 characters. The maximum Unicode UTF-8 string is 16,000 bytes (octets). 
The maximum single-byte or multi-byte string is 32,000 bytes.

A Unicode UTF-16 string can be coerced to a UTF-8 string. A Unicode UTF-16 string can be coerced to either a 
single-byte string or a multi-byte string as long as the character represented in the UTF-16 string exists in the 
local character set implied by the II_CHARSETxx setting.

**Note:** Ingres and OpenROAD support the Unicode Basic Multi-Lingual Plane with the exception of the Unicode 
Surrogate range. A Unicode Surrogate pair is treated as two independent Unicode code points.

For details about Unicode Normalization Forms, go to http://www.unicode.org.

For more information about character transliteration with SessionObject.GetEnv method, see GetEnv Method.
