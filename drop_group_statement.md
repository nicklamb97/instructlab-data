Drop Group Statement
The drop group statement removes the specified group identifiers from the installation. If any of the specified identifiers does not exist, the DBMS Server returns an error but does not abort the statement. Other valid existing group_ids in the statement are deleted.
This statement has the following syntax:
drop group group_id {, group_id};
A group identifier must be empty, that is, have no users in its user list, before it can be dropped. If an attempt is made to drop a group identifier that still has members in its user list, the DBMS Server returns an error and does not delete the identifier. However, the statement is not aborted. Other group identifiers in the list, if they are empty, are deleted. (Use the alter group statement to drop all the users from a group's user list.)
Any session using a group identifier when the identifier is dropped continues to run with the privileges defined for that group.
For more information about group identifiers, see the Ingres Database Administrator Guide.
Parameters--Drop Group Statement
This statement has the following parameter:
group_id
Specifies the group to be dropped. A group identifier must be empty, that is, have no users in its user list, before it can be dropped.
Drop Group Permissions
You must have MAINTAIN_USERS privilege and be connected to the iidbdb database.
Drop Group Locking
The drop group statement locks pages in the iiusergroup catalog of the iidbdb.
Drop Group Related Statements
Alter Group Statement
Create Group Statement
Examples--Drop Group Statement
The following are drop group statement examples:
1.Drop the group identifier, acct_clerk.
drop group acct_clerk;
2.In an application, drop the group identifiers, tel_sales and temp_clerk.
drop group tel_sales, temp_clerk;
