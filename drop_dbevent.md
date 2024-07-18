# Drop Dbevent Statement

The drop dbevent statement drops the specified database event.

## Syntax

```
drop dbevent [schema.]event_name;
```

## Parameters

This statement has the following parameter:

- **event_name**: Specifies the name of the event to drop.

## Permissions

You must be the owner of a database event. If applications are currently registered to receive the database event, the registrations are not dropped. If the database event was raised prior to being dropped, the database event notifications remain queued, and applications can receive them using the get dbevent statement.

## Related Statements

- Create Dbevent Statement
- Grant (privilege) Statement
- Raise Dbevent Statement
- Register Dbevent Statement

## Examples

The following statement deletes the `drill_hot` dbevent:

```
drop dbevent drill_hot;
```
