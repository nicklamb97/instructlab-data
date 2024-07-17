Drop Rule Statement
The drop rule statement removes the specified rule from the database. (A rule is dropped automatically if the table on which the rule is defined is dropped.)
This statement has the following syntax:
drop rule [schema.]rulename;
Parameters--Drop Rule Statement
This statement has the following parameter:
rulename
Specifies the name of the rule to drop
Drop Rule Permissions
You must be the owner of a rule.
Examples--Drop Rule Statement
The following example drops the chk_name rule:
drop rule chk_name;
