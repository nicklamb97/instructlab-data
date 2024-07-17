Modify Statement
The modify statement changes properties of a table or index.
This statement has the following syntax:
modify [schema.]table_name|[schema.]indexname |
              [ schema.]table-name
              to modify-action [unique]
              [on column_name [asc|desc]{, column_name [asc|desc]}]
              [with_clause];
Parameters--Modify Statement
This statement has the following parameters:
modify-action
Specifies how the table should be modified. The modify-action can be any one of the following keywords:
isam
Modifies the table storage structure to the ISAM structure.
hash
Modifies the table storage structure to the HASH structure
heap
Modifies the table storage structure to the HEAP structure
heapsort
Modifies the table storage structure to the HEAP structure, and additionally sort the rows in the table as directed
btree
Modifies the table storage structure to the BTREE structure
reconstruct
Modifies the table storage structure to what it currently is (the table is physically rebuilt)
truncated
Truncates the table, deleting all data
reorganize
Moves the data to a different location
relocate
Moves the table to a different location
merge
Shrinks a btree index
add_extend
Adds disk pages to the table
[no]readonly
Marks the table read only or not read only
phys_[in]consistent
Marks the table physically consistent or inconsistent
log_[in]consistent
Marks the table logically consistent or inconsistent
table_recovery_[dis]allowed
Allows or disallow table level rollforward
[no]persistence
Marks the index to be recreated automatically as needed (secondary indexes only)
table_debug
Displays internal table data structures
encrypt
Enables or disables encryption for the table. Must be used with the with passphrase clause. For more information on data encryption, see the Ingres Security Guide.
The additional action_keywords cheap, chash, cisam, and cbtree are accepted. cheap is a synonym for heap with compression=(data), and the others similarly. These forms are deprecated; the with compression= clause should be used instead.
One of the storage structure actions (heap, hash, isam, btree) can be used instead of reconstruct.
unique
Requires each key value in the restructured table to be unique. This clause is used only with the isam, hash, or btree modify-actions.
on column-name
Determines the storage structure keys of the table. This clause is used only with isam, hash, btree, or heapsort actions.
with_clause
Specifies with clause parameters consisting of the word with followed by a comma-separated list of any number of the following items:
•allocation = n
•extend = n
•fillfactor = n  (isam, hash, and btree only)
•minpages = n  (hash only)
•maxpages = n  (hash only)
•leaffill = n  (btree only)
•nonleaffill = n  (btree only)
•blob_extend = n  (btree only)
•newlocation = (locationname {, locationname})
•oldlocation = (locationname {, locationname})
•location = (locationname {, locationname})
•compression [= ([[NO]KEY] [,[NO|HI]DATA])] | NOCOMPRESSION
•[NO]PERSISTENCE
•unique_scope = ROW | STATEMENT
•page_size = n
•priority = cache_priority
•nopartition
•partition = (partitioning-scheme)
•concurrent_updates
•passphrase = encryption_passphrase
     [new_passphrase = new encryption_passphrase]
Modify Description
The modify statement enables the following operations to be performed:
•Change the storage structure of the specified table or index
•Specify the number of pages allocated for a table or index, and the number of pages by which it grows when it requires more space
•Add pages to a table
•Reorganize a btree index
•Move a table or index, or portion thereof, from one location to another
•Spread a table over many locations or consolidate a table onto fewer locations
•Delete all rows from a table and release its file space back to the operating system
•Specify whether an index is recreated when its base table is modified
•Specify how unique columns are checked during updates: after each row is inserted or after the update statement is completed
•Mark table as physically or logically consistent or inconsistent
•Mark table as allowed/disallowed for table-level recovery
•Defer uniqueness check until the end of statement execution
•Mark a table as read only
•Assign a table fixed cache priority
•Change a table's partitioning scheme
•Enable modify table to be performed online
•Enable or disable access to a table with encrypted columns
•Change the encryption passphrase used to access encrypted data
You can change a table's location and storage structure in the same modify statement.
The modify statement operates on existing tables and indexes. When modifying a table to change, truncate, or reconstruct the storage structure, the DBMS Server destroys any indexes that exist for the specified table (unless the index was created with persistence, or the table is a btree and the table being modified to reorganize its index).
Syntax for Modify Operations
Use the syntax shown below to perform the listed operation:
•Reorganize a btree table's index:
modify table_name|indexname to merge
•Move a table:
modify table_name|indexname to relocate
    with oldlocation = (locationname {, locationname}),
        newlocation = (locationname {, locationname}),
•Change locations for a table:
modify table_name|indexname to reorganize
    with location = (locationname {, locationname})
•Delete all data in a table:
modify table_name|indexname to truncated
•Add pages to a table:
modify table_name|indexname to add_extend
    [with extend = number_of_pages]
where:
number_of_pages is 1 to 8,388,607.
•Add pages to blob extension table:
modify table_name|indexname to modify-option
    with blob_extend [,extend = number_of_pages]
where:
number_of_pages is 1 to 8,388,607.
•Mark a table as physically consistent or inconsistent:
modify table_name|indexname to phys_consistent|phys_inconsistent
•Mark a table as logically consistent or inconsistent:
modify table_name|indexname to log_consistent|log_inconsistent
•Mark a table as allowed or disallowed for table-level recovery:
modify table_name|indexname to
    table_recovery_allowed|table_recovery_disallowed
•Defer the uniqueness check until the end of statement execution:
modify table_name to modify-action with unique_scope = statement
•Mark a table as readonly:
modify table_name to [no]readonly
•Assign a table fixed cache priority:
modify table_name to modify-action with priority = cache_priority
•Change a table's partitioning scheme:
modify table_name to reconstruct
with partition = (partitioning-scheme)
•Enable a table modification to be performed online:
modify table_name to modify-option with concurrent_updates
•Enable access to a table with encrypted columns:
modify table_name to encrypt
with passphrase='encryption passphrase'
•Disable access to a table with encrypted columns:
modify table_name to encrypt
with passphrase=''
•Change the passphrase for a table with encrypted columns:
modify table_name to encrypt
with passphrase='encryption passphrase',
    new_passphrase='new encryption passphrase'
Storage Structure Specification
Changing the storage structure of a table or index is typically done to improve performance when accessing the table. For example, to improve the performance of copy, change the structure of a table to heap before performing a bulk copy into the table.
The storage_structure parameter must be one of the following:
isam
Indexed Sequential Access Method structure, duplicate rows allowed unless the with noduplicates clause is specified when the table is created. Maximum key width is 1003 bytes.
hash
Random hash storage structure, duplicate rows allowed unless the with noduplicates clause is specified when the table is created. Maximum key width is 32000 bytes.
heap
Unkeyed and unstructured, duplicated rows allowed, even if the with noduplicates clause is specified when the table is created.
heapsort
Heap with rows sorted and duplicate rows allowed unless the WITH NODUPLICATES clause is specified when the table is created (sort order not retained if rows are added or replaced).
btree
Dynamic tree-structured organization with duplicate rows allowed unless the with noduplicates clause is specified when the table is created. Maximum key width depends on the page_size:
Page Size
Maximum Key Width in Bytes
2k
440
4K
500
8K
1000
16K
2000
32K
2000
64K
2000
An index cannot be modified to heap, heapsort, or rtree.
The DBMS Server uses existing data to build the index (for isam and btree tables), calculate hash values (for hash tables) or for sorting (heapsort tables).
To optimize the storage structure of heavily used tables (tables containing data that is frequently added to, changed, or deleted), modify those tables periodically.
The optional keyword unique requires each key value in the restructured table to be unique. (The key value is the concatenation of all key columns in a row.) If UNIQUE is specified on a table that contains non-unique keys, the DBMS Server returns an error and does not change the table's storage structure. For the purposes of determining uniqueness, a null is considered to be equal to another null. Use unique only with isam, hash, and btree tables.
The optional on clause determines the table's storage structure keys. This clause can only be specified when modifying to one of the following storage structures: isam, hash, heapsort, or btree. When the table is sorted after modification, the first column specified in this clause is the most significant key, and each successive specified column is the next most significant key.
If the on clause is omitted when modifying to isam, hash, or btree, the table is keyed, by default, on the first column. When a table is modified to heap, the on clause must be omitted.
When modifying a table to heapsort, specify the sort order as asc (ascending) or desc (descending). The default is asc. When sorting, the DBMS Server considers nulls greater than any non‑null value.
In general, any modify...to storage_structure ... of a table or index assigned to a raw location must include with location=(...) syntax to move the table or index to another set of locations because modify semantics involve a create, rename, and delete file, which works efficiently for cooked locations, but does not adapt to raw locations.
If unique is used with a partitioned table, the new storage structure must be compatible with the table's partitioning scheme. This means that given a value for the unique storage structure key columns, it must be possible to determine that a row with that key will be in one particular physical partition. In practice, this rule means that the partitioning scheme columns must be the same as (or a subset of) the storage structure key columns. It also means that unique cannot be used with automatic partitioning. A modify to unique that violates the partitioning rule will be rejected with an error.
Note:  It is still possible to enforce an arbitrary uniqueness constraint on a partitioned table, regardless of the partitioning scheme, by adding a unique or primary key constraint, or a unique secondary index, to the table.
Modify...to Reconstruct
To rebuild the existing storage structure for a table or partition, use the modify...to reconstruct option.
The reconstruct action allows the table or partitions to be rebuilt, maintaining the existing storage structure, key columns, and storage attributes. Any overrides specified in the modify statement with clause are applied.
The reconstruct variant provides a simple means for partitioning or unpartitioning a table without affecting its other attributes. Partitioning or unpartitioning a table requires rewriting its storage structure, which is why partitioning is limited to restructuring variants of the modify statement.
The heapsort structure is not really a storage structure, in the sense that the sort criteria are not remembered in any system catalog. Therefore reconstruction of a table originally modified to heapsort simply remodifies the table to heap with no additional sorting.
When operating on specific logical partitions instead of an entire table, the reconstruct modify does not permit any override with attributes except for the location option.
The partition name clause allows the modify statement to operate on specific named logical partitions. Partition names must be listed from outer dimension to inner dimension.
Modify...to Merge
To shrink a btree index, use the modify...to merge option. When data is added to a btree table, the index automatically expands. However, a btree index does not shrink when rows are deleted from the btree table.
Modify...to merge affects only the index, and therefore usually runs a good deal faster than the other modify variants. Modify...to merge does not require any temporary disk space to execute.
Modify...to Relocate
To move the data without changing the number of locations or storage structure, specify modify...to relocate.
For example, to relocate the employee table to three different areas:
modify employee to relocate
        with    oldlocation = (area1, area2, area3),
                newlocation = (area4, area5, area6);
The data in area1is moved to area4, the data in area2 is moved to area5, and the data on area3 is moved to area6. The number of areas listed in the oldlocation and newlocation options must be equal. The data in each area listed in the oldlocation list is moved “as is” to the corresponding area in the newlocation list. The oldlocation and newlocation options can only be used when relocate is specified.
To change some but not all locations, specify only the locations to be changed. For example, you can move only the data in area1 of the employee table:
modify employee to relocate
    with    oldlocation = (area1),
            newlocation = (area4);
Areas 2 and 3 are not changed.
The DBMS Server is very efficient at spreading a table or index across multiple locations. For example, if a table is to be spread over three locations:
create table large (wide varchar(2000))
        with location = (area1, area2, area3);
rows are added to each location in turn, in 16 page (approximately 32K for the 2K page size) chunks. If it is not possible to allocate 16 full pages on an area when it is that area's turn to be filled, the table is out of space, even if there is plenty of room in the table's other areas.
Modify...to Reorganize
To move the data and change the number of locations without changing storage structure, specify modify...to reorganize. For example, to spread an employee table over three locations:
modify employee to reorganize
with location = (area1, area2, area3);
When specifying reorganize, the only valid with clause option is location.
Modify...to Truncated
To delete all the rows in the table and release the file space back to the operating system, specify modify...to truncated. For example, the following statement deletes all rows in the acct_payable table and releases the space:
modify acct_payable to truncated;
Using truncated converts the storage structure of the table to heap. The with_clause options cannot be specified when using modify...to truncated.
Modify...to Add_extend
To add pages to a table, specify modify...to add_extend. To specify the number of pages to be added, use the extend=number_of_pages option. If the with extend=number_of_pages option is omitted, the DBMS Server adds the default number of pages specified for extending the table. To specify the default, use the modify...with extend statement. If no default has been specified for the table, 16 pages are added.
The modify...to add_extend option does not rebuild the table or drop any secondary indexes.
Modify...with Blob_extend
To add pages to a blob extension table, specify modify...with blob_extend. To specify the number of pages to be added, use the extend=number_of_pages option. If the with extend=number_of_pages option is omitted, the DBMS Server adds the default number of pages specified for extending the table. To specify the default, use the modify...with extend statement. If no default has been specified for the table, 16 pages are added.
The modify...with blob_extend option does not rebuild the table or drop any secondary indexes.
Modify...to Phys_consistent|Phys_inconsistent
A physically inconsistent table has some form of physical corruption. A table is marked physically inconsistent if rollforwarddb of that table fails for some reason, or if the table is a secondary index and point-in-time recovery has occurred only on the base table.
The modify...to phys_consistent command marks the table as physically consistent but does not change the underlying problem.
Modify...to Log_consistent|Log_inconsistent
A logically inconsistent table is out-of-step in some way with one or more logically related tables. A table is marked logically inconsistent if the table is journaled and the user enters rollforwarddb +c -j on the table, or if the table is journaled and the user rolls forward to a point-in-time. For example, if two tables are logically related through referential constraints, and only one is moved to a specific point-in-time, the table is marked logically inconsistent.
The modify to log_consistent command marks the table as logically consistent but does not fix the underlying problem.
Modify...to Table_recovery_allowed|Table_recovery_disallowed
To prevent the possibility of logically or physically inconsistent tables due to table-level recovery, table-level recovery can be disallowed for any table with the modify...to table_recovery_disallowed command.
Modify…to [No]Readonly
This option marks the table readonly and returns an error if insert, delete, or update operations are performed. Additionally, all transactions that use the read only table take a shared table lock.
With Clause Options for Modify
The remaining with clause options for the modify statement are described below.
Fillfactor, Minpages, and Maxpages
Fillfactor specifies the percentage (from 1 to 100) of each primary data page that must be filled with rows, under ideal conditions. For example, if you specify a fillfactor of 40, the DBMS Server fills 40% of each of the primary data pages in the restructured table with rows. You can specify this option with the isam, hash, or btree structures. Take care when specifying large fillfactors because a non-uniform distribution of key values can later result in overflow pages and thus degrade access performance for the table.
Minpages specifies the minimum number of primary pages a hash table must have. Maxpages specifies the maximum number of primary pages a hash table can have. Minpages and maxpages must be at least 1. If both minpages and maxpages are specified in a modify statement, minpages must not exceed maxpages.
For best performance, the values for minpages and maxpages must be a power of 2. If a number other than a power of 2 is chosen, the DBMS Server can change the number to the nearest power of 2 when the modify executes. To ensure that the specified number is not changed, set both minpages and maxpages to that number.
By default, modify to storage-structure resets these attributes back to their defaults (listed below). The modify to reconstruct operation does not affect these attributes.
Default values for fillfactor, minpages, and maxpages are listed in this table:
Fillfactor
Minpages
Maxpages
Hash
50
16
no limit
Compressed hash
75
1
no limit
Isam
80
n/a
n/a
Compressed isam
100
n/a
n/a
Btree
80
n/a
n/a
Compressed btree
100
n/a
n/a
Leaffill and Nonleaffill
For btree tables, the leaffill parameter specifies how full to fill the leaf index pages. Leaf index pages are the index pages that are directly above the data pages. Nonleaffill specifies how full to fill the non-leaf index pages; non-leaf index pages are the pages above the leaf pages. Specify leaffill and nonleaffill as percentages. For example, if you modify a table to btree, specifying nonleaffill=75, each non-leaf index page is 75% full when the modification is complete.
The leaffilland nonleaffill parameters can assist with controlling locking contention in btree index pages. If some open space is retained on these pages, concurrent users can access the btree with less likelihood of contention while their queries descend the index tree. Strike a balance between preserving space in index pages and creating a greater number of index pages. More levels of index pages require more I/O to locate a data row.
By default, modify to storage-structure resets these attributes back to their defaults.
Default: leaffill=70; nonleaffill=80
The modify to reconstruct operation does not affect these attributes.
Allocation
Use the with allocation option to specify the number of pages initially allocated to the table or index. By pre-allocating disk space to a table, runtime errors that result from running out of disk space can be avoided. If the table is spread across multiple locations, space is allocated across all locations.
The number of pages specified must be between 4 and 8,388,607 (the maximum number of pages in a table). If the specified number of pages cannot be allocated, the modify statement is aborted.
A table can be modified to a smaller size. If the table requires more pages that you specify, the table is extended and no data is lost. A table can be modified to a larger size to reserve disk space for the table.
If not specified, a modify does not change a table's allocation.
Extend
To specify the number of pages by which a table or index grows when it requires more space, use the with extend clause. The number of pages specified must be between 1 and 8,388,607 (the maximum number of pages in a table). If the specified number of pages cannot be allocated when the table must be extended (for example, during an insert operation), the DBMS Server aborts the statement and issues an error. By default, tables and indexes are extended by groups of 16 pages.
If not specified, a modify does not change a table's extend attribute.
Compression
To specify data and key compression, use the with compression clause. Compression removes the string trim from variable character data. The following table lists valid compression options:
Storage Structure
Base Table or
Secondary Index
Can Compress Data?
Can Compress Key?
Hash
Base Table
Yes
No
 
Secondary Index
Yes
No
Heap
Base Table
Yes
No
 
Secondary Index
No
No
Btree
Base Table
Yes
Yes
 
Secondary Index
No
Yes
Isam
Base Table
Yes
No
 
Secondary Index
Yes
No
To specify an uncompressed storage structure, specify with nocompression.
To compress both key and data for tables where this is valid (primarily btree), specify with compression, omitting the key and data clause. To compress data or keys independently of one another, specify with compression = (key|data). To compress data using bit compression, specify with compression = hidata. To explicitly suppress compression of data or keys, specify with compression = (nokey | nodata).
If not specified, modify to storage-structure removes compression, unless the c-prefix variants are used (cbtree and so on). Other variants of modify preserve the table's compression type.
If a secondary index is compressed (with key compression), the non-key columns will also be compressed.
Location
To change the location of a table when modifying its storage structure, specify the location option. This option specifies one or more new locations for the table. The locations specified must exist when the statement executes and the database must have been extended to those locations. For information about areas and extending databases, see the Ingres Database Administrator Guide.
Unique_scope
The unique_scope option specifies, for tables or indexes with unique storage structures, how uniqueness is checked during an update option.
There are two options:
unique_scope = ROW
Checks uniqueness as each row is inserted.
unique_scope = STATEMENT
Checks uniqueness after the update statement is completed.
Default: unique_scope = ROW, when first imposing uniqueness on a table.
Specify the unique_scope option only when modifying to a unique storage structure. For example:
modify mytable to btree unique with unique_scope = ROW;
If not otherwise specified, a modify does not change the unique_scope setting.
(No)Persistence
The [no]persistence option specifies whether an index is recreated when its related base table is modified. This option is valid only for indexes.
There are two options:
with persistence
Recreates the index when its base table is modified.
with nopersistence
Drops the index when its base table is modified.
Default: A modify to a storage structure sets an index to nopersistence.
Other modify actions (including modify to reconstruct) do not change an index's persistence.
Page_size
Specify page size using page_size = n where n can be the page size in the following table:
Page Size
Number of Bytes
2K
2,048
4K
4,096
8K
8,192
16K
16,384
32K
32,768
64K
65,536
The default page size is 2,048. The tid size is 4. The buffer cache for the installation must also be configured with the page size you specify in create table or an error occurs.
The page size of a session temporary table cannot be changed by a modify.
Nopartition | Partition=
The partition= clause allows the partitioning scheme of a table to be changed. The table does not have to be partitioned initially. The nopartition clause removes partitioning from a table. For the syntax of a partition= specification, see Partitioning Schemes in the Ingres SQL Reference Guide.
The with_clause options partition= or nopartition are permitted in a modify statement only if the modify statement specifies a storage structure which includes the reconstruct action. Other forms of the modify statement (for example, modify to truncated) do not allow either partition= or nopartition clauses.
The default for the modify statement is to not change the partitioning scheme of a table.
Concurrent_updates
The concurrent_updates option specifies that a table modify is to be performed online. Unlike a regular modify, which locks out all other access to the table for the entire duration, an online modify permits normal read and update access to the table for most of the modify.
When the online modify completes, however, exclusive access to the table is still required during a brief period to apply any updates that were made to the table during the online modify. The length of this period depends on the number of updates that must be applied.
Online modification of tables cannot be accomplished in the following:
•Ingres clusters
•Temporary tables
•System catalogs
•Partitioned tables
•Secondary indexes
Note:  To use online modification, a database must be journaled.
Nodependency_check
A table cannot be modified if it destroys indexes needed for constraints. The operation can be forced, however, by using the nodependency_check option.
Important!  If you use this option, you must preserve or recreate the table structure necessary to enforce the constraints.
Passphrase=
Passphrase specifies an encryption passphrase used to access a table with one or more encrypted columns. The passphrase must match the latest one defined for the table. With passphrase must be used with the encrypt modify action.
The syntax of the with passphrase clause is:
with passphrase='encryption passphrase', [new_passphrase='new encryption passphrase']
New_passphrase lets you change the passphrase. (The passphrase is initially defined on the create table statement.) It must be preceded by the with passphrase clause. The passphrase must be at least eight characters, can contain spaces, and must be enclosed in single quotes.
After the new passphrase is defined using new_passphrase, access to the encrypted table is disabled until an additional modify … encrypt with passphrase statement is issued, which verifies that the new passphrase was typed correctly.
If the second command succeeds you can then issue a commit, confident that you have changed the passphrase correctly. If this series of modify … encrypt … new_passphrase and modify … encrypt commands fails because of a password mismatch, you can retry the second modify (if you mistyped the passphrase) or roll back the passphrase change attempt to the old passphrase and try again.
Modify Permissions
You must own the table or have SECURITY privilege and connect as the owner.
Modify Locking
The modify statement requires an exclusive table lock. Other sessions, even those using readlock=NOLOCK, cannot access the table until the transaction containing the modify statement is committed.
Two exceptions are the modify to table_debug variant, which takes a shared table lock, and the concurrent_updates option, which takes only a brief exclusive lock at the end of the modify operation.
Modify Related Statements
Copy Statement
Create Table Statement
Examples--Modify Statement
The following are modify statement examples:
1.Modify the employee table to an indexed sequential storage structure with eno as the keyed column.
modify employee to isam on eno;
If eno is the first column of the employee table, the same result can be achieved by:
modify employee to isam;
2.Redo the isam structure on the employee table, but request a 60% occupancy on all primary pages.
modify employee to reconstruct
        with fillfactor = 60;
3.Modify the job table to compressed hash storage structure with jid and salary as keyed columns.
modify job to hash on jid, salary
        with compression;
4.Perform the same modify, but also request 75% occupancy on all primary pages, a minimum of 7 primary pages, and a maximum of 43 primary pages.
modify job to hash on jid, salary
        with compression, fillfactor = 75,
        minpages = 7, maxpages = 43;
5.Perform the same modify again but request a minimum of 16 primary pages.
modify job to hash on jid, salary
        with compression, minpages = 16;
6.Modify the dept table to a heap storage structure and move it to a new location.
modify dept to heap with location=(area4);
7.Modify the dept table to a heap again, but sort rows on the dno column and remove any duplicate rows.
modify dept to heapsort on dno;
8.Modify the employee table in ascending order by ename, descending order by age, and remove any duplicate rows.
modify employee to heapsort on ename asc,
        age desc;
9.Modify the employee table to btree on ename so that data pages are 50% full and index pages are initially 40% full.
modify employee to btree on ename
with fillfactor = 50, leaffill = 40;
10.Modify a table to btree with data compression, no key compression. This is the format used by the (obsolete) cbtree storage structure.
modify table1 to btree
        with compression=(nokey, data);
11.Modify an index to btree using key compression.
modify index1 to btree with compression=(key);
12.Modify an index so it is retained when its base table is modified. Its current table structure is unchanged.
modify empidx to reconstruct with persistence;
13.Modify a table, specifying the number of pages to be initially allocated to it and the number of pages by which it is extended when it requires more space.
modify inventory to btree
        with allocation = 10000, extend = 1000;
14.Modify an index to have uniqueness checked after an UPDATE statement completes.
modify empidx to btree unique on empid
        with unique_scope = statement;
15.Remove partitioning from a table.
modify lineitems to reconstruct with nopartition;
16.Modify the table to disable encryption and decryption (which prevents all table access).
modify secrets to encrypt with passphrase='';
17.Modify the encrypted table to re-enable access.
modify secrets to encrypt with passphrase='to encrypt or not encrypt, that is the question';
18.Modify the encrypted table's passphrase, and immediate verify and commit the new passphrase.
MODIFY SECRETS TO ENCRYPT WITH PASSPHRASE='to encrypt or not encrypt, that is the question', new_passphrase='now is the passphrase of our discontent changed';
modify secrets to encrypt with passphrase='now is the passphrase of our discontent changed';
commit;
