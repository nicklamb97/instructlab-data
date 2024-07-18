# Date Functions

OpenROAD supports functions that derive values from absolute dates and from interval dates. These functions operate on rows or variables that contain date values. An additional function, dow(), returns the day of the week (for example, mon, tue) for a specified date. The dow() function is described in Data Type Conversion Functions.

The following date functions extract the specified portion of a date, time, or timestamp. They can be used with the ingresdate data types.

- YEAR(): Extracts the year portion of a date.
- QUARTER(): Extracts the quarter corresponding to the date. Quarters are numbered 1 through 4.
- MONTH(): Extracts the month portion of a date.
- WEEK(): Extracts the number of the week of the year that the date refers to. Week 1 begins on the first Monday of the year. Dates before the first Monday of the year are considered to be in week 0. Weeks are numbered 1 to 53.
- WEEK_ISO(): Extracts the number of the week of the year that the date refers to, and conforms to ISO 8601 definition for number of the week. Week_iso begin on Monday, but the first week is the week that has the first Thursday of the year. If you are using ISO-Week and the date falls before the week containing the first Thursday of the year, that date is considered part of the last week of the previous year, and date_part returns either 52 or 53.
- DAY(): Extracts the day portion of a date.
- HOUR(): Extracts the hour portion of a time.
- MINUTE(): Extracts the minute portion of a time.
- SECOND(): Extracts the second portion of a time.
- MICROSECOND(): Extracts the fractions of seconds portion of a time as microseconds.
- NANOSECOND(): Extracts the fractions of seconds portion of a time as nanoseconds.

Examples:
- DAY('2006-12-15 12:30:55.1234') returns 15
- SECOND('2006-12-15 12:30:55.1234') returns 55.1234

## Date Functions for Date(ingresdate) Data Type

Note: The functions described in this section apply only to the date(ingresdate) data type.

SQL supports functions that derive values from absolute dates and from interval dates. These functions operate on columns that contain date values. An additional function, DOW(), returns the day of the week (mon, tue, and so on) for a specified date. For a description of the DOW() function, see Data Type Conversion Functions.

Some date functions require specifying of a unit parameter; unit parameters must be specified using a quoted string. The following lists valid unit parameters:

- Second: SECOND, SECONDS, SES, SECS
- Minute: MINUTE, MINUTES, MIN, MINS
- Hour: HOUR, HOURS, HR, HRS
- Day: DAY, DAYS
- Week: WEEK, WEEKS, WK, WKS
- ISO-Week: ISO-WEEK, ISO-WK
- Month: MONTH, MONTHS, MO, MOS
- Quarter: QUARTER, QUARTERS, QTR, QTRS
- Year: YEAR, YEARS, YR, YRS

The following lists the date functions:

1. DATE_TRUNC(unit,date)
   - Format (Result): date
   - Description: Returns a date value truncated to the specified unit.

2. DATE_PART(unit,date)
   - Format (Result): integer
   - Description: Returns an integer containing the specified (unit) component of the input date.

3. DATE_GMT(date)
   - Format (Result): any character data type
   - Description: Converts an absolute date into the Greenwich Mean Time character equivalent with the format yyyy_mm_dd hh:mm:ss GMT. If the absolute date does not include a time, the time portion of the result is returned as 00:00:00.

   For example, the query:
   ```sql
   SELECT DATE_GMT('1-1-98 10:13 PM PST')
   ```
   returns the following value:
   ```
   1998_01_01 06:13:00 GMT
   ```
   while the query:
   ```sql
   SELECT DATE_GMT('1-1-1998')
   ```
   returns:
   ```
   1998_01_01 00:00:00 GMT
   ```

4. GMT_TIMESTAMP(s)
   - Format (Result): any character data type
   - Description: Returns a twenty-three-character string giving the date s seconds after January 1, 1970 GMT. The output format is 'yyyy_mm_dd hh:mm:ss GMT'.

   For example, the query:
   ```sql
   SELECT (GMT_TIMESTAMP (1234567890))
   ```
   returns the following value:
   ```
   2009_02_13 23:31:30 GMT
   ```
   while the query:
   ```sql
   (II_TIMEZONE_NAME = AUSTRALIA-QUEENSLAND)
   SELECT date(GMT_TIMESTAMP (1234567890))
   ```
   returns:
   ```
   14-feb-2009 09:31:30
   ```

5. INTERVAL (unit,date_interval)
   - Format (Result): float
   - Description: Converts a date interval into a floating-point constant expressed in the unit of measurement specified by unit. The interval function assumes that there are 30.436875 days per month and 365.2425 days per year when using the mos, qtrs, and yrs specifications.

   For example, the query:
   ```sql
   SELECT(INTERVAL('days', '5 years'))
   ```
   returns the following value:
   ```
   1826.213
   ```

6. ISDST(date)
   - Format (Result): integer
   - Description: Returns 1 if date falls within Daylight Saving Time for the session timezone, else 0.

7. _DATE(s)
   - Format (Result): any character data type
   - Description: Returns a nine-character string giving the date s seconds after January 1, 1970 GMT. The output format is dd-mmm-yy.

   For example, the query:
   ```sql
   SELECT _DATE(123456)
   ```
   returns the following value:
   ```
   2-jan-70
   ```
   Note that this function formats a leading space for day values less than 10.

8. _DATE4(s)
   - Format (Result): any character data type
   - Description: Returns an eleven-character string giving the date s seconds after January 1, 1970 GMT. The output format is controlled by the II_DATE_FORMAT setting.

   For example, with II_DATE_FORMAT set to US, the query:
   ```sql
   SELECT _DATE4(123456)
   ```
   returns the following value:
   ```
   02-jan-1970
   ```
   while with II_DATE_FORMAT set to MULTINATIONAL, the query:
   ```sql
   SELECT _DATE4(123456)
   ```
   returns the following value:
   ```
   02/01/1970
   ```

9. _TIME(s)
   - Format (Result): any character data type
   - Description: Returns a five-character string giving the time s seconds after January 1, 1970 GMT, which is then adjusted for your local time zone. The output format is hh:mm (seconds are truncated).

   For example, the query:
   ```sql
   SELECT _TIME(123456)
   ```
   returns the value 02:17 for the NA-PACIFIC time zone.

## Truncate Dates using DATE_TRUNC Function

Use the DATE_TRUNC function to group all the dates within the same month or year, and so forth. For example:

```sql
DATE_TRUNC('month',date('23-oct-1998 12:33'))
```
returns 1-oct-1998, and
```sql
DATE_TRUNC('year',date('23-oct-1998'))
```
returns 1-jan-1998.

Truncation takes place in terms of calendar years and quarters (1-jan, 1-apr, 1-jun, and 1-oct).

To truncate in terms of a fiscal year, offset the calendar date by the number of months between the beginning of your fiscal year and the beginning of the next calendar year (6 mos for a fiscal year beginning July 1, or 4 mos for a fiscal year beginning September 1):

```sql
DATE_TRUNC('year',date+'4 mos') - '4 mos'
```

Weeks start on Monday. The beginning of a week for an early January date falls into the previous year.

## Using DATE_PART

The DATE_PART function is useful in set functions and in assuring correct ordering in complex date manipulation. For example, if date_field contains the value 23-oct-1998, then:

```sql
DATE_PART('month',date(date_field))
```
returns a value of 10 (representing October), and
```sql
DATE_PART('day',date(date_field))
```
returns a value of 23.

- Months are numbered 1 to 12, starting with January.
- Hours are returned according to the 24-hour clock.
- Quarters are numbered 1 through 4.
- Week 1 begins on the first Monday of the year. Dates before the first Monday of the year are considered to be in week 0. However, if you specify ISO-Week, which is ISO 8601 compliant, the week begins on Monday, but the first week is the week that has the first Thursday. The weeks are numbered 1 through 53.

Therefore, if you are using Week and the date falls before the first Monday in the current year, date_part returns 0. If you are using ISO-Week and the date falls before the week containing the first Thursday of the year, that date is considered part of the last week of the previous year, and DATE_PART returns either 52 or 53.

The following illustrates the difference between Week and ISO-Week:

- Date Column: 02-jan-1998, Day of Week: Fri, Week: 0, ISO-Week: 1
- Date Column: 04-jan-1998, Day of Week: Sun, Week: 0, ISO-Week: 1
- Date Column: 02-jan-1999, Day of Week: Sat, Week: 0, ISO-Week: 53
- Date Column: 04-jan-1999, Day of Week: Mon, Week: 1, ISO-Week: 1
- Date Column: 02-jan-2000, Day of Week: Sun, Week: 0, ISO-Week: 52
- Date Column: 04-jan-2000, Day of Week: Tue, Week: 1, ISO-Week: 1
- Date Column: 02-jan-2001, Day of Week: Tue, Week: 1, ISO-Week: 1
- Date Column: 04-jan-2001, Day of Week: Thu, Week: 1, ISO-Week: 1

## DATE_FORMAT

DATE_FORMAT(datetime, format)

TIME_FORMAT(datetime, format)

TIME_FORMAT is an alias for DATE_FORMAT.

Operand type: datetime is a DATE, TIME, or TIMESTAMP; format is a character string
Result type: VARCHAR

Returns datetime formatted according to the format string.

The specifiers in the following can be used in the format string. The "%" character is required before format specifier characters. If the format specifier is inappropriate to the data type of datetime, then NULL is returned.

- %a: Abbreviated weekday name (Sun..Sat)
- %b: Abbreviated month name (Jan..Dec)
- %c: Month, numeric (0..12)
- %D: Day of the month with English suffix (1st, 2nd, 3rd, …)
- %d: Day of the month, numeric (01‑31)
- %e: Day of the month, numeric (1‑31)
- %f: Microseconds (000000..999999)
- %H: Hour (00..23)
- %h: Hour (01..12)
- %I: Hour (01..12)
- %i: Minutes, numeric (00..59)
- %j: Day of year (001..366)
- %k: Hour (0..23)
- %l: Hour (1..12)
- %M: Month name (January..December)
- %m: Month, numeric (00..12)
- %p: AM or PM
- %r: Time, 12-hour (hh:mm:ss followed by AM or PM)
- %S: Seconds (00..59)
- %s: Seconds (00..59)
- %T: Time, 24-hour (hh:mm:ss)
- %U: Week (00..53), where Sunday is the first day of the week
- %u: Week (00..53), where Monday is the first day of the week
- %V: Week (01..53), where Sunday is the first day of the week; used with %X
- %v: Week (01..53), where Monday is the first day of the week; used with %x
- %W: Weekday name (Sunday..Saturday)
- %w: Day of the week (0=Sunday..6=Saturday)
- %X: Year for the week where Sunday is the first day of the week, numeric, four digits; used with %V
- %x: Year for the week, where Monday is the first day of the week, numeric, four digits; used with %v
- %Y: Year, numeric (four digits)
- %y: Year, numeric (two digits)
- %%: A literal "%" character
- %x: x, for any "x" not listed above

Examples:

```sql
DATE_FORMAT('2010-10-03 22:23:00', '%W %M %Y')
```
returns 'Sunday October 2010'

```sql
DATE_FORMAT('2007-10-03 22:23:00', '%H:%i:%s')
```
returns '22:23:00'

```sql
DATE_FORMAT('1900-10-04 22:23:00', '%D %y %a %d %m %b %j')
```
returns '4th 00 Thu 04 10 Oct 277'

```sql
DATE_FORMAT('1997-10-04 22:23:00', '%H %k %I %r %T %S %w')
```
returns '22 22 10 10:23:00 PM 22:23:00 00 6'

```sql
DATE_FORMAT('1999-01-01', '%X %V')
```
returns '1998 52'
