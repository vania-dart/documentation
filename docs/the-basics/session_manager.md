---
sidebar_label: 'Session Manager'
sidebar_position: 16
---

# Session Manager

The Session Manager helps you store, retrieve, and remove session data across multiple requests. You can also configure how long sessions should last by setting a session lifetime value in your `.env` file.

## Session Lifetime

In your `.env` file, you can add or update the following line to set the session lifetime (in seconds):

```
SESSION_LIFETIME=86400
```

This example sets the session to expire after **24 hours** (86,400 seconds).

## Setting Session Data

Use `setSession` to store data in the session. Since it's asynchronous, call it with `await`:

```dart
await setSession('userId', 123);
```

Here, `'userId'` is the key and `123` is the value. You can store any kind of data.

## Retrieving Session Data

Use `getSession` to read data from the session:

```dart
var userId = await getSession<int?>('userId');

var allSession = await allSessions();

```

If the key doesn't exist, it returns `null`. Make sure to handle that in your code.

## Deleting a Specific Key

Use `deleteSession` to remove a single session key:

```dart
await deleteSession('userId');
```

This deletes only the data associated with `'userId'`.

## Deleting All Session Data

Use `destroyAllSessions` to clear all session data at once:

```dart
await destroyAllSessions();
```
