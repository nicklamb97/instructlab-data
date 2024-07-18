# Create Table Statement

The `CREATE TABLE` statement creates a base table in Ingres.

**Syntax:**
```sql
create table [schema.]table_name
             (column_specification {, column_specification})
             [with with_clause];
```

The `CREATE TABLE...AS SELECT` statement creates a table and loads rows from another table.

**Syntax:**
```sql
create table table_name
             (column_name {, column_name}) as
             subselect
             {union [all]
             subselect}
             [with with_clause];
```

## Parameters
- **table_name**: Defines the name of the new table.
- **column_specification**: Defines characteristics of columns.
- **with_clause**: Comma-separated options for table properties.

## Create Table Description
The `CREATE TABLE` statement creates a base table that stores rows independently.

- Default page size is determined by installation configuration or specified using `page_size`.
- Maximum of 1024 columns allowed per table.
- Long varchar and long byte columns can hold up to 2 GB.

## Column Specification
The format for defining columns in `CREATE TABLE`:
```sql
column_name datatype
[with null | not null]
[with default default_spec | with default | not default]
```

### Default Clause
- **not default**: Mandatory column.
- **with default**: Insert default value when none provided.
- **with default default_spec**: Use specific default value.

### Null Clause
- **with null**: Column accepts null values.
- **not null**: Column does not accept null values.

### System_Maintained Logical Keys
Columns marked `SYSTEM_MAINTAINED` are assigned values by the DBMS.

## Create Table...As Select
Creates a table populated with rows from another table or tables.

- Storage structure defaults to heap with compression.
- Column names and data types derived from source table.

## With_Clause for Create Table
Options for `with_clause` in `CREATE TABLE`:
- **location = (location_name {, location_name})**
- **[no]journaling**
- **[no]duplicates**
- **page_size = n**
- **security_audit = (audit_opt {, audit_opt})**
- **security_audit_key = (column)**
- **[no]autostruct**
- **encryption = AES128|AES192|AES256, aeskey=hex-aes-key, passphrase='encryption-passphrase'**

## With_Clause for Create Table...As Select
Options for `with_clause` in `CREATE TABLE...AS SELECT`:
- **allocation = n**
- **extend = n**
- **structure = HASH | HEAP | ISAM | BTREE**
- **key = (column_name {, column_name})**
- **fillfactor = n**
- **minpages = n**
- **maxpages = n**
- **compression[=([[NO]KEY] [, [NO]DATA])] | NOCOMPRESSION**
- **leaffill = n**
- **nonleaffill = n**
- **priority = n**

## Create Table Permissions
Available to all users. Controlled using `GRANT` statement.

## Create Table Locking
DBMS takes an exclusive table lock during creation.

## Create Table Related Statements
- Alter Table Statement
- Drop Table Statement
- Grant (privilege) Statement
- Modify Statement

## Examples
1. Create `employee` table with journaling enabled:
```sql
create table employee
   (eno smallint,
    ename varchar(20) not null with default,
    age integer,
    job smallint,
    salary float4,
    dept smallint)
   with journaling;
```
   
2. Create a table `highincome` listing employee numbers for high earners:
```sql
create table highincome AS
    select eno
    from employee
    where salary > (select avg(salary) from employee);
```

3. Create `emp` table spanning two locations with specified allocation:
```sql
create table emp as
    select eno from employee
    with location = (location1, location2),
    allocation = 1000;
```

4. Create `dept` table with specified defaults:
```sql
create table dept (
    dname char(10),
    location char(10) not null with default 'LA',
    creation_date date not null with default '1/1/99',
    budget money not null with default 100000,
    expenses money not null with default 0);
```

5. Create `department` table structured as hash unique on `dept_id`:
```sql
create table department (dept_id char(6) not null, dept_name char(20));
modify department to hash unique on dept_id;
```
