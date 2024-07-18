# Create Dbevent Statement

The `create dbevent` statement defines a database event.

## Syntax

```
create dbevent [schema.]event_name;
```

The `create dbevent` statement creates the specified database event. Database events enable an application to pass status information to other applications.

Database events can be registered or raised by any session, provided that the owner has granted the required permission (raise or register) to the session's user, group, or role identifier, or to public. Only the user, group, or role that created a database event can drop that database event.

## Parameters

### event_name
Identifies the event. The `event_name` must be a valid object name.

## Permissions

### All Users
This statement is available to all users.

## Create Dbevent Locking

The `create dbevent` statement locks pages in the `iievent` catalog.

## Create Dbevent Related Statements

- **Drop Dbevent Statement**
- **Grant (privilege) Statement**
- **Raise Dbevent Statement**
- **Register Dbevent Statement**
