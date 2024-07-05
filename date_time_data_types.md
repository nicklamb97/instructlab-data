## Date/Time Data Types

Date/time data types in OpenROAD primarily focus on date data types.

### Background on Date Data Types

Ingres and OpenROAD applications have traditionally used the INGRESDATE (previously named DATE) data type, which allows storage of:

- Empty dates (to match empty varchar strings)
- Date-only values (e.g., 1901-02-03)
- Date/time values accurate to a second (e.g., 1901-02-03 04:05:06)
- Interval values (e.g., "2 days", "2 mins")

ANSI SQL Standard later introduced distinct data types for these, except for empty dates. Ingres and OpenROAD maintain support for INGRESDATE while adding support for new types.

### Date/Time Input Formats

OpenROAD supports two main data type input formats:
1. INGRESDATE
2. DATE (Note: In OpenROAD, DATE and INGRESDATE are interchangeable)

### Date(ingresdate) Data Types

The date(ingresdate) data type is an abstract data type that can contain:

1. Absolute date input
2. Absolute time input
3. Combined date and time input
4. Date interval
5. Time interval

#### Absolute Date Input

Date format is determined by the II_DATE_FORMAT setting. The default is US format. Some examples of II_DATE_FORMAT settings include:

- US: mm/dd/yyyy
- MULTINATIONAL: dd/mm/yy
- ISO: yyyymmdd
- GERMAN: dd.mm.yyyy

#### Absolute Time Input

Format: `hh:mm[:ss] [am|pm] [timezone]`

- Uses 24-hour clock
- If timezone is omitted, local time zone is assumed
- Times are stored as GMT and displayed using II_TIMEZONE_NAME adjustment

#### Combined Date and Time Input

Combines valid absolute date and time formats. Examples:
- 11/15/03 10:30:00
- 15-nov-03 10:30:00 gmt
- 15-nov-03 10:30:00 am

#### Date Interval

Specified in terms of years, months, days, or combinations. Examples:
- '5 years'
- '8 months 14 days'
- '5 yrs 8 mos 14 days'

#### Time Interval

Specified as hours, minutes, seconds, or combinations. Examples:
- '23 hours'
- '38 minutes 53 secs'
- '23:38:53 hours'

### Date and Time Display

- Displays as 25-character strings with trailing blanks
- Output format specified by II_DATE_FORMAT
- Time intervals display most significant portions that fit in 25 characters

### Coercion Between Date/Time Data Types

OpenROAD 6.2 supports:
- Ingres DATE (INGRESDATE or DATE) data type
- Coercion of various Ingres ANSI INTERVAL, TIME, DATE, and TIMESTAMP columns into OpenROAD DATE variables
- Selection, update, and insertion of INGRESDATE, ANSI DATE, TIME, and TIMESTAMP string literals to target DBMS

Note: When coercing INGRES TIMESTAMP to INGRESDATE, milliseconds are truncated. When coercing INGRESDATE to INGRES TIMESTAMP, the result lacks millisecond precision.
