# Alter Group Statement

The alter group statement modifies the list of users associated with a group identifier. Individual users can be added or dropped or the entire list can be dropped. An add and a drop operation cannot be performed in the same alter group statement.

This statement has the following syntax:

```sql
alter group group_id {, group_id}
    add users (user_id {, user_id})
    | drop users (user_id {, user_id})
    | drop all;
```

## Parameters--Alter Group Statement

This statement has the following parameters:

### alter group *group_id* {, *group_id*}

Modifies the list of users associated with a group identifier (*group_id*). The *group_id* must be an existing group identifier. If a non-existent *group_id* is specified, a warning is issued and processing continues with the next valid *group_id* in the list.

### add users (*user_id* {, *user_id*})

Adds the specified *user_id*s to the specified the *group_id*s. The *user_id*s must exist to be added to a group. If a specified *user_id* does not exist, an error is issued and processing continues with the remaining *user_id*s. If a specific *user_id* occurs more than once in the user list, additional occurrences of the specified *user_id* are ignored. No errors are issued.

### drop users (*user_id* {, *user_id*})

Removes the specified *user_id*s to the specified the *group_id*s. If a specified *user_id* is not in the group user list, an error is issued and processing continues with the remaining *user_id*s. A user cannot be dropped from a group if that group is the default group of the user. Use the alter user statement to change a user's default group. If a user is dropped from a group in a session that is associated with that group, the user retains the privileges of the group until the session terminates.

### drop all

Removes all users from the specified *group_id*s. If any member of the specified group has that group as its default group, drop all results in an error. Use the alter user statement to change the user's default group. If a user is dropped from a group in a session that is associated with that group, the user retains the privileges of the group until the session terminates.

## Permissions

Maintain_users privilege is required to execute the alter group statement.

## Locking

The alter group statement locks pages in the iiusergroup catalog in the iidbdb. This causes sessions attempting to connect to the server to be suspended until the alter group statement is completed.

## Related Statements

- Create Group Statement
- Drop Group Statement

## Examples--Alter Group Statement

The following examples add and drop user identifiers from the user list associated with a group identifier:

1. Add users to the group, sales_clerks.

```sql
alter group sales_clerks
    add users (dannyh, helent);
```

2. Drop three users from the group, tel_sales.

```sql
alter group tel_sales
    drop users (harryk, joanb, elainet);
```

3. In an application, drop all users from the group, researchers.

```sql
alter group researchers drop all;
```
