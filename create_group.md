# Create Group Statement

The `create group` statement establishes a group identifier and associates it with the specified list of users. Group identifiers enable the database administrator (or user that has the security privilege) to grant identical privileges to a group of users. For a complete discussion of group identifiers and their use, see the Ingres Database Administrator Guide.

After creating a group identifier and specifying its members, the system administrator can grant privileges to the group identifier. When a member of the group begins a session, the group identifier can be specified in the `sql` or `connect` statement (or on the operating system command line, using the `-G` flag) to obtain the privileges associated with the group. For more information, see the Ingres SQL Reference Guide.

## Syntax

```
create group group_id {, group_id}
              [with users = (user_id {, user_id})];
```

## Parameters

### group_id
Is the group identifier. It must be a valid object name that is unique among all user, group, and role identifiers in the installation. If an invalid identifier is specified in the list of group identifiers, the DBMS Server returns an error but processes all valid group identifiers. Group identifier names are stored in the `iiusergroup` catalog in the `iidbdb` database.

### user_id
Must be a valid user name. If an invalid user identifier is specified, the DBMS Server issues an error but processes all valid user identifiers. A group can contain any number of users. A group identifier can be created without specifying a user list. To add users to an existing group identifier, use the `alter group` statement (see Alter Group Statement).

## Create Group Permissions

You must have `MAINTAIN_USERS` privilege and be connected to the `iidbdb` database.

## Create Group Locking

The `create group` statement locks pages in the `iiusergroup` catalog in the `iidbdb`. This can cause sessions attempting to connect to the server to be suspended until the `create group` statement is completed.

## Create Group Related Statements

- Alter Group Statement
- Drop Group Statement

## Examples

### Example 1
Create a group identifier for the telephone sales force of a company and put the user IDs of the salespeople in the user list of the group.

```sql
create group tel_sales with users = (harryk, joanb, jerryw, arlenep);
```

### Example 2
In an application, create a group identifier for the inventory clerks of a store and place their user IDs in the user list of the group.

```sql
create group inv_clerk with users = (jeanies, louisem, joep);
```
