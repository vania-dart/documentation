---
sidebar_label: 'Caching'
sidebar_position: 8
---

# Caching

## Introduction

Caching is essential for improving the efficiency of data retrieval processes that are CPU-intensive or time-consuming. By storing data temporarily, your application can quickly retrieve this data for subsequent requests. Typical use cases include caching one-time passwords (OTP), database results, and other frequently accessed data.

## Configuration

The caching configuration is found in `lib/config/app.dart`.

## Cache Usage

### Obtaining a Cache Instance

To use the caching features, obtain an instance of the `Cache` class. This class provides straightforward access to the Vania cache mechanisms:

```dart
class UserController extends Controller
{
    Future<Response> index() async
    {
        String? value = Cache.get('key');
        return Response.json();
    }
}
```

## Retrieving Items From the Cache

Retrieve items using the `get` method. If the item does not exist, `null` is returned:

```dart
dynamic value = Cache.get('key');
```

## Determining Item Existence

Use the `has` method to check if an item is present in the cache. This method returns `false` if the item is either non-existent or its value is `null`:

```dart
bool exists = Cache.has('key');
```

## Storing Items in the Cache

To store items, use the `put` method. Items can be set to expire after a default duration of one hour or a specified duration:

```dart
await Cache.put('otp', otp.toString(), duration: Duration(minutes: 3));
```

## Storing Items Permanently

The `forever` method stores items indefinitely. Such items must be manually removed with the `delete` method:

```dart
await Cache.forever('key', 'value');
```

## Removing Items From the Cache

Remove items using the `delete` method:

```dart
await Cache.delete('key');
```

Items can also be effectively removed by setting their expiration to zero or a negative duration:

```dart
await Cache.put('key', 'value', duration: Duration(microseconds: 0));
```
