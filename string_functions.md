# String Functions

String functions perform a variety of operations on character data. String functions can be nested. For example:

```sql
LEFT(RIGHT(x.name, SIZE(x.name) - 1), 3)
```

returns the substring of x.name from character positions 2 through 4, and

```sql
CONCAT(CONCAT(x.lastname, ', '), x.firstname)
```

concatenates x.lastname with a comma and concatenates x.firstname with the first concatenation result.

The + operator can also be used to concatenate strings:

```sql
x.lastname + ', ' + x.firstname
```

## String Functions and the UTF-8 Character Set

Note: For the UTF-8 character set, the character data is multibyte string and the actual number of bytes for the data could be more than the number of characters. If the output buffer for any string operation is not sufficiently large to hold the multibyte string, the result will be truncated at a character boundary.

## String Functions Supported

The following table lists the string functions supported in SQL.

The expressions c1 and c2, representing function arguments, can be any of the string types (char, varchar, long varchar, c, text, byte, varbyte, long varbyte) or any of the Unicode types (nchar, nvarchar, long nvarchar), except where noted. The expressions len, n, n1, n2 or nshift, representing function arguments, are the integer type. For string functions operating on one of the string types, the integer arguments represent character (or 8-bit octet) counts or offsets. For string functions operating on one of the Unicode types, the integer arguments represent "code point" (or 16-bit Unicode characters) counts or offsets.

- **ASCII(v1)**
  - Result Type: any character type
  - Description: Returns the character equivalent of the value v1, which is an expression of either character or numeric type.

- **CHAREXTRACT(c1,n)**
  - Result Type: varchar or nchar
  - Description: Returns the nth character or code point of c1. If n is larger than the length of the string, the result is a blank character. It does not support long varchar or long nvarchar arguments.

- **CHARACTER_LENGTH(c1)**
  - Result Type: integer
  - Description: Returns the number of characters in c1 without trimming blanks, as is done by the LENGTH() function. This function does not support nchar and nvarchar arguments.

- **CHR(n)**
  - Result Type: character
  - Description: Converts integer into corresponding ASCII code. If n is greater than 255, the conversion is performed on n mod 256.

- **CONCAT(c1,c2)**
  - Result Type: any character or Unicode data type, byte
  - Description: Concatenates one string to another. The result size is the sum of the sizes of the two arguments. If the result is a c or char string, it is padded with blanks to achieve the proper length. To determine the data type results of concatenating strings, see the table regarding results of string concatenation. This function does not support long nvarchar arguments.

- **GENERATE_DIGIT('scheme', 'c1')**
  - Result Type: varchar(2)
  - Description: Generates the check digit for the string c1 using the specified encoding scheme. Supported schemes are described under VALIDATE_DIGIT. The check digit is computed from the other characters in the string. Although the majority of check digits are a single character, some encoding schemes may return a two character check digit. Typically the check digit is appended to the string for validation, but this is not the case in all encoding schemes. This function does not append the check digit to the string.

The check digit can be used to help determine if a string has been entered correctly. A check digit may be able to detect several types of errors, including:
- Mistyped characters ('0123' not '0124')
- Omissions or additions of characters
- Adjacent transposition errors ('01234' not '01243')
- Twin errors ('11' becomes '22')
- Jump transpositions (123 becomes 321)
- Jump twin errors (121 becomes 323)
- Phonetic errors—the mixing of similar sounding numbers (sixty '60' not sixteen '16')

A check digit improves the chance of detecting errors in keying, but is not an infallible error detector. Furthermore, a check digit does not provide error correction capability.

There are many check digit schemes, each with its own purpose, capabilities, and complexity. For example, the Luhn algorithm is widely used in credit card numbers and is programmatically simple to implement. The Verhoeff algorithm, in contrast, is better at detecting certain error types such as jump transpositions, but is more complicated.

The scheme name is case insensitive. When specifying the LUHN or VERHOEFF scheme, the function can take as input a character string of any length. The other schemes are fixed length, so the input string must be of the correct length.

This function does not support nchar and nvarchar arguments.

- **INITCAP(c1)**
  - Result Type: any character or Unicode data type
  - Description: Converts all initial characters in c1 to upper case.

```sql
SELECT INITCAP('This is the final version (VERSION:5.a;6) of Leonard''s will')
```

returns 'This Is The Final Version (Version:5.A;6) Of Leonard's Will'

- **LEFT(c1,len)**
  - Result Type: any character or Unicode data type
  - Description: Returns the leftmost len characters of c1. If the result is a fixed-length c or char string, it is the same length as c1, padded with blanks. The result format is the same as c1. This function does not support long nvarchar arguments.

- **LENGTH(c1)**
  - Result Type: smallint (for long varchar, returns 4-byte integer)
  - Description: If c1 is a fixed-length c or char string, returns the length of c1 without trailing blanks. If c1 is a variable-length string, returns the number of characters actually in c1.

- **LOCATE(c1,c2)**
  - Result Type: smallint
  - Description: Returns the location of the first occurrence of c2 within c1, including trailing blanks from c2. The location is in the range 1 to size(c1). If c2 is not found, the function returns size(c1) + 1. The function size() is described below, in this table. If c1 and c2 are different string data types, c2 is coerced into the c1 data type. This function does not support long varchar or long nvarchar arguments.

- **LOWERCASE(c1)** or **LOWER(c1)**
  - Result Type: any character or Unicode data type
  - Description: Converts all upper case characters in c1 to lower case.

- **LPAD(expr1, n [, expr2])**
  - Result Type: any character type
  - Description: Returns character expression of length n in which expr1 is prepended by n-m blanks (where m is length(expr1)) or, if expr2 is coded, enough copies of expr2 to fill n-m positions at the start of the result string.

```sql
SELECT LPAD ('Company',20, '-')
```

returns '-------------Company'

- **LTRIM(expr)**
  - Result Type: any character data type
  - Description: Returns character expression with leading blanks removed.

- **OCTET_LENGTH(c1)**
  - Result Type: integer
  - Description: Returns the number of 8-bit octets (bytes) in c1 without trimming blanks, as is done by the LENGTH() function.

- **PAD(c1)**
  - Result Type: text, varchar, or nvarchar
  - Description: Returns c1 with trailing blanks appended to c1. For example, if c1 is a varchar string that could hold 50 characters but only has two characters, then PAD(c1) appends 48 trailing blanks to c1 to form the result.

- **RIGHT(c1,len)**
  - Result Type: any character or Unicode data type
  - Description: Returns the rightmost len characters of c1. Trailing blanks are not removed first. If c1 is a fixed-length character string, the result is padded to the same length as c1. If c1 is a variable-length character string, no padding occurs. The result format is the same as c1. This function does not support long nvarchar arguments.

- **RPAD(expr1, n [, expr2])**
  - Result Type: any character type
  - Description: Returns character expression of length n in which expr1 is appended by n-m blanks (where m is length(expr1)) or, if expr2 is coded, enough copies of expr2 to fill n-m positions at the end of the result string.

```sql
SELECT RPAD('Company',12, '-')
```

returns 'Company-----'

```sql
SELECT RPAD('Company',12, '-x')
```

returns 'Company-x-x-'

- **RTRIM(expr)**
  - Result Type: any character data type
  - Description: Returns character expression with trailing blanks removed.

- **SHIFT(c1,nshift)**
  - Result Type: any character or Unicode data type
  - Description: Shifts the string nshift places to the right if nshift > 0 and to the left if nshift < 0. If c1 is a fixed-length character string, the result is padded with blanks to the length of c1. If c1 is a variable-length character string, no padding occurs. The result format is the same as c1. This function does not support long varchar or long nvarchar arguments.

- **SIZE(c1)**
  - Result Type: smallint
  - Description: Returns the declared size of c1 without removal of trailing blanks.

- **SOUNDEX(c1)**
  - Result Type: any character data type
  - Description: Returns a c1 four-character field that can be used to find similar sounding strings. For example, SMITH and SMYTHE produce the same SOUNDEX code. If there are less than three characters, the result is padded by trailing zero(s). If there are more than three characters, the result is achieved by dropping the rightmost digits. This function is useful for finding like-sounding strings quickly. A list of similar sounding strings can be shown in a search list rather than just the next strings in the index. This function does not support long varchar or Unicode arguments.

- **SOUNDEX_DM(c1)**
  - Result Type: any character type
  - Description: Returns one or more six-character codes in a comma-separated varchar string (up to a maximum of 16 codes) that can be used to find similar sounding names. This Daitch-Mokotoff soundex function returns one or more six-character codes in a comma-separated varchar string (up to a maximum of 16 codes) that can be used to find similar sounding names. For example: the codes 739460,734600 may be returned for the name Peterson.

Leading and embedded spaces are ignored in the input. The input strings are terminated by non-alphabetic characters other than period (.), apostrophe ('), or hyphen (-). The function ignores the first occurrence of any of these characters, but terminates the string on a subsequent occurrence. This allows the input of names or place names with standard punctuation marks. For example:

- O'Brien is treated as 'OBRIEN'
- St.Kilda is treated as 'STKILDA'
- St. Hilda is treated as 'STHILDA'
- 'Smyth-Brown' is treated as 'SMYTHBROWN'

An empty name, or a name with characters unable to be encoded, returns a code of 000000 and no error is generated.

This function does not support long varchar or Unicode arguments.

For more information, see SOUNDEX_DM vs. SOUNDEX).

- **SQUEEZE(c1)**
  - Result Type: text or varchar
  - Description: Compresses white space. White space is defined as any sequence of blanks, null characters, newlines (line feeds), carriage returns, horizontal tabs and form feeds (vertical tabs). Trims white space from the beginning and end of the string, and replaces all other white space with single blanks. This function is useful for comparisons. The value for c1 must be a string of variable‑length character string data type (not fixed-length character data type). The result is the same length as the argument. This function does not support long varchar or long nvarchar arguments.

- **SUBSTR(c1, loc [, len])**
  - Result Type: varbyte, varchar, nvarchar, long variants
  - Description: Returns part of c1 starting at the loc position and either extending to the end of the string or for the number of characters/code points in the len operand. If len is specified and is less than 1, SUBSTR returns NULL. The loc parameter determines the start of the substring to be extracted. If loc is less than 0 the position is counted backwards from the end of c1; if loc is greater than 0 the position is counted from the beginning. If loc is 0 the start position is the first character. After the start of the substring is determined, len characters are extracted. If len is omitted, the rest of c1 is implied. If c1 is varchar, char, c, or text, the result is a varchar. If c1 is nchar or nvarchar, the result is an nvarchar. If c1 is a byte or varbyte, the result is a varbyte. If c1 is passed as a long data type, the result is of the same format.

```sql
SELECT SUBSTR('Company 2012',9,2)
```

returns '20'

```sql
SELECT SUBSTR('Company 2012',9)
```

returns '2012'

```sql
SELECT SUBSTR('Company 2012',-9,4)
```

returns 'pany'

- **TRIM(c1)**
  - Result Type: text or varchar
  - Description: Returns c1 without trailing blanks. The result has the same length as c1. This function does not support long varchar or long nvarchar arguments.

- **UPPERCASE(c1)** or **UPPER(c1)**
  - Result Type: any character data type
  - Description: Converts all lower case characters in c1 to upper case.

- **VALIDATE_DIGIT('scheme', c1)**
  - Result Type: integer
  - Description: Validates that the check digit in the string c1 is mathematically correct for the specified scheme. The function returns 1 for valid and 0 for invalid. Restrictions on scheme names and strings are the same as for GENERATE_DIGIT. You must not include separators, such as dashes or whitespace, in the string. The VALIDATE_DIGIT function does not enforce any internal formatting rules of the scheme. For example, an EAN is split into a GS1 Prefix, the Company Number, an Item reference, and the check digit. This function only calculates the check digit.

The scheme for GENERATE_DIGIT and VALIDATE_DIGIT functions is case insensitive and can be one of the following:

- EAN - European Article Number. Synonyms: EAN_13, GTIN, GTIN_13, JAN. The EAN is a bar-coding standard defined by the standards organization GS1. It is used worldwide for marking retail goods. It was developed as a superset of the original 12-digit Universal Product Code (UPC) system. It is also called a Japanese Article Number (JAN) in Japan. UPC, EAN, and JAN numbers are collectively called Global Trade Item Numbers (GTIN). An EAN string is a 13-character string composed entirely of digits. The check digit is the rightmost character in the string.

- EAN_8 - European Article Number. Synonyms: GTIN_8, RCN_8. The EAN_8 is an 8-character number derived from the longer EAN_13 and is intended for use on small packages where the longer number would be awkward. EAN_8 can also be used internally by companies to identify restricted or "own-brand" products to be sold within their stores only. These are often referred to with the synonym RCN_8. An EAN_8 string is an 8-character string composed entirely of digits. The check digit is the rightmost character in the string.

- ISBN - International Standard Book Number. The standard ISBN is a 10-character alphanumeric identifier for printed material. The check digit is the rightmost character and can be either a digit or the alpha 'X'. The other characters in the string must be digits.

- ISBN_13 - International Standard Book Number. ISBN-13 is the 13-character version of the ISBN. The string is composed entirely of digits. The check digit is the rightmost character in the string.

- ISSN - International Standard Serial Number. The ISSN is an 8-character alphanumeric identifier for electronic or print periodicals. The check digit is the rightmost character and can be either a digit or the alpha 'X'. The other characters in the string must be digits.

- **LUHN**: The LUHN algorithm. Most credit cards and many government identification numbers are based on this simple algorithm created by Hans Peter Luhn. For example, the Canadian Social Security number is a nine digit number based on the Luhn algorithm. The string can be of any permitted length, but must be composed entirely of digits. The check digit is the rightmost character in the string.

- **LUHN_A**: The LUHN algorithm (alphanumeric). LUHN_A is an extension of the Luhn algorithm that allows for alphabetic characters by assigning numbers 10 to 'A', 11 to 'B', ...35 to 'Z'. It is case insensitive. The Committee on Uniform Security Identification Procedures (CUSIP) provides a 9-character alphanumeric string based on this algorithm. Also the International Securities Identification Number (ISIN) is a 12-character version of this algorithm. The string can be of any permitted length, but must be composed entirely of alphanumeric characters. The check digit is the rightmost character in the string.

- **UPC**: Universal Product Code. Synonyms: UPC_A, EAN_12. Used to mark retail goods as per the EAN (see above). The UPC is composed of 12 digits with the check digit being the rightmost character.

- **UPC_E**: Universal Product Code. The UPC_E is composed of six digits and is intended for smaller packages where a full size UPC_A would be cumbersome. Unlike the UPC_A, the check digit is not appended to the number, but is used to determine the "odd/even" parity assigned to the numbers for when they are encoded as barcode lines.

- **VERHOEFF**: The Verhoeff algorithm. This algorithm detects all transposition errors and other types of errors that are not detected by the Luhn algorithm. Like the Luhn algorithm, it can handle strings of any permitted length. The string must be composed entirely of digits and the check digit will be the rightmost character.

- **VERHOEFFNR**: Verhoeff algorithm as described in Numerical Recipes in C. A variation on the Verhoeff algorithm published in Numerical Recipes in C by Press, Teukolsky, Vetterling, and Flannery.

# SOUNDEX_DM vs. SOUNDEX

The advantages of the Daitch-Mokotoff soundex over the standard (Russell) soundex function are:

- The extra length of the codes returned avoids false positives on long names with the same base.

For example:

Name: Nichols
SOUNDEX Code: N242
SOUNDEX_DM Code: 658400,648400

Name: Nicholson
SOUNDEX Code: N242
SOUNDEX_DM Code: 658460,648460

In this example the soundex() function fails to differentiate between the names, whereas the SOUNDEX_DM() function returns several codes for each name, none of which match any elements in the other.

- The ability to return multiple codes allow for the different sounds certain character combinations make.

For example: Does the "ch" in Cherkassy sound like the "ch" in cheese or in Christmas?

- The ability to return multiple codes allow a greater chance to match names that the standard soundex will miss.

For example:

Name: Schwartsenegger
SOUNDEX Code: S632
SOUNDEX_DM Code: 479465

Name: Shwarzenegger
SOUNDEX Code: S625
SOUNDEX_DM Code: 479465,474659

Name: Schwarzenegger
SOUNDEX Code: S625
SOUNDEX_DM Code: 479465,474659

In this example, the soundex() function fails to match the misspelling 'Schwartsenegger', whereas the SOUNDEX_DM() function generates a string with a matching element (479465) for the other two cases listed.

The disadvantage of the Daitch-Mokotoff soundex over the standard soundex function is the computational overhead required to process a multi-element string.

# String Concatenation Results

The following shows the results of concatenating expressions of various character data types:

1st String: c, 2nd String: c, Trim Blanks from 1st?: Yes, Trim Blanks from 2nd?: --, Result Type: C
1st String: c, 2nd String: text, Trim Blanks from 1st?: Yes, Trim Blanks from 2nd?: --, Result Type: C
1st String: c, 2nd String: char, Trim Blanks from 1st?: Yes, Trim Blanks from 2nd?: --, Result Type: C
1st String: c, 2nd String: varchar, Trim Blanks from 1st?: Yes, Trim Blanks from 2nd?: --, Result Type: C
1st String: text, 2nd String: c, Trim Blanks from 1st?: No, Trim Blanks from 2nd?: --, Result Type: C
1st String: char, 2nd String: c, Trim Blanks from 1st?: Yes, Trim Blanks from 2nd?: --, Result Type: C
1st String: varchar, 2nd String: c, Trim Blanks from 1st?: No, Trim Blanks from 2nd?: --, Result Type: C
1st String: text, 2nd String: text, Trim Blanks from 1st?: No, Trim Blanks from 2nd?: No, Result Type: text
1st String: text, 2nd String: char, Trim Blanks from 1st?: No, Trim Blanks from 2nd?: Yes, Result Type: text
1st String: text, 2nd String: varchar, Trim Blanks from 1st?: No, Trim Blanks from 2nd?: No, Result Type: text
1st String: char, 2nd String: text, Trim Blanks from 1st?: Yes, Trim Blanks from 2nd?: No, Result Type: text
1st String: varchar, 2nd String: text, Trim Blanks from 1st?: No, Trim Blanks from 2nd?: No, Result Type: text
1st String: char, 2nd String: char, Trim Blanks from 1st?: No, Trim Blanks from 2nd?: --, Result Type: char
1st String: char, 2nd String: varchar, Trim Blanks from 1st?: No, Trim Blanks from 2nd?: --, Result Type: char
1st String: varchar, 2nd String: char, Trim Blanks from 1st?: No, Trim Blanks from 2nd?: --, Result Type: char
1st String: varchar, 2nd String: varchar, Trim Blanks from 1st?: No, Trim Blanks from 2nd?: No, Result Type: varchar
1st String: nchar, 2nd String: nchar, Trim Blanks from 1st?: No, Trim Blanks from 2nd?: No, Result Type: nchar
1st String: nchar, 2nd String: nvarchar, Trim Blanks from 1st?: No, Trim Blanks from 2nd?: No, Result Type: nchar
1st String: nvarchar, 2nd String: nchar, Trim Blanks from 1st?: No, Trim Blanks from 2nd?: No, Result Type: nchar
1st String: nvarchar, 2nd String: nvarchar, Trim Blanks from 1st?: No, Trim Blanks from 2nd?: No, Result Type: nvarchar

When concatenating more than two operands, expressions are evaluated from left to right. For example:

```
varchar + char + varchar
```

is evaluated as:

```
(varchar+char)+varchar
```

To control concatenation results for strings with trailing blanks, use the TRIM and PAD functions.
