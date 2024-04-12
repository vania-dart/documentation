---
id: requests
title: Requests
sidebar_position: 4
---

## Introduction

The Requests class in Vania offers a convenient way to work with the incoming HTTP requests that your application
receives. It acts as a bridge between your application and the data sent by the client's browser.

With the Requests class, you can easily access information about the current request, such as the data submitted by the
user, any files uploaded with the request, and other relevant details.

By utilizing the Requests class, developers can efficiently retrieve and process user input, handle file uploads, and
perform various operations based on the incoming request data. This object-oriented approach simplifies the handling of
HTTP requests within your application, enabling you to build responsive and interactive web experiences without
unnecessary complexity.

## Accessing the Request

To obtain an instance of the current HTTP request via dependency injection, you should type-hint the Request class on
your route closure or controller method. The incoming request instance will automatically be injected

```dart
class UserController extends Controller {
  Future store(Request request) {
    var name = request.input('name');
    return Response.json({'name': name});
  }
}
```

As mentioned, you may also type-hint the Request class on a route closure.

```dart
Route.get('/', (Request request) {
  // ...
});
```

## Request Method

### Retrieving the Request Path

The path method returns the request's path information. So, if the incoming request is targeted
at [http://example.com/foo/bar](http://example.com/foo/bar), the path method will return `foo/bar`:

```dart
Route.get('/', (Request request) {
  final path = request.path;
});
```

### Retrieving the Request Host

You may retrieve the `host` of the incoming request via the host, httpHost

```dart
Route.get('/', (Request request) {
  final host = request.host;
});
```

### Retrieving the Request Url

To retrieve the URL for the incoming request you may use the url methods. The url method will return the URL without the
query string

```dart
Route.get('/', (Request request) {
  final url = request.url;
});
```

### Retrieving the Request Method

The method will return the HTTP verb for the request. You may use the isMethod method to verify that the HTTP
verb matches a given string:

```dart
Route.get('/', (Request request) {
  var mesthod = request.method;

  if (request.isMethod('post')) {
    // ...
  }
});
```

### Request Headers

You may retrieve a request header from the Request If the header is not present on the request, null will be returned.
However, the header method accepts an optional second argument that will be returned if the header is not present on the
request

```dart
Route.get('/', (Request request) {
  final header = request.header('X-Header-Name');
  final header = request.header('X-Header-Name', 'Defualt Header');
});
```

### Request IP Address

The ip method may be used to retrieve the IP address of the client that made the request to your application:

```dart
Route.get('/', (Request request) {
  final ip = request.ip;
});
```

## Retrieving Input

### Retrieving All Input Data

You may retrieve all the incoming request's input data as a List using the all method. This method may be used
regardless of whether the incoming request is from an HTML form or is an XHR request:

```dart
final allRequest = request.all();
```

### Retrieving an Input Value

Using a few simple methods, you may access all the user input from your Request instance without worrying about which
HTTP verb was used for the request. Regardless of the HTTP verb, the input method may be used to retrieve user input:

```dart
final name = request.input('name');
```

You may pass a default value as the second argument to the input method. This value will be returned if the requested
input value is not present on the request:

```dart
final name = request.input('name','Vania');
```

You may call the input method without any arguments in order to retrieve all the input values as an associative list:

```dart
final name = request.input();
```

### Retrieving Input From the Query String

While the input method retrieves values from the entire request payload (including the query string), the query method
will only retrieve values from the query string:

```dart
final query = request.query('name')
```

If the requested query string value data is not present, the second argument to this method will be returned:

```dart
final defaultQuery = request.query('name', 'John')
```

You may call the query method without any arguments in order to retrieve all the query string values as an
associative list:

```dart
final all = request.query()
```

### Retrieving String Input Values

Instead of retrieving the request's input data as a primitive string, you may use the string method to retrieve the
request data as String

```dart
String name = request.string('name');
```

### Retrieving Boolean Input Values

```dart
bool archived = request.boolean('archived');
```

### Retrieving DateTime Input Values

```dart
DateTime date = request.boolean('create_at');
```

### Retrieving a Portion of the Input Data

If you need to retrieve a subset of the input data, you may use the only and except methods. Both of these methods
accept a single list or a dynamic list of arguments:

```dart
final input = request.only(['username', 'password']);

final input = request.except(['credit_card']);

final input = request.except('credit_card');
```

The only method returns all the key / value pairs that you request; however, it will not return key / value pairs
that are not present on the request.

### Input Presence

You may use the has method to determine if a value is present on the request. The has method returns true if the value
is present on the request:

```dart
if (request.has('name')) {
  // ...
}
```

When given a list, the has method will determine if all the specified values are present:

```dart
if (request.has(['name', 'age'])) {
  // ...
}
```

The hasAny method returns true if any of the specified values are present:

```dart
if (request.hasAny(['name', 'age'])) {
  // ... 
}
```

A second closure may be passed to the whenHas method that will be executed if the specified value is not present on the
request:

```dart
request
    .wheeHas('name')
    .then((input) {
      // ...
    })
    .onError((error, stackTrace) {
      // ...
    });
```

### Merging Additional Input

Sometimes you may need to manually merge additional input into the request's existing input data. To accomplish this,
you may use the merge method. If a given input key already exists on the request, it will be overwritten by the data
provided to the merge method:

```dart
request.merge({'age': 55})
```

## Retrieving Uploaded Files

You may retrieve uploaded files from a Request, The file method returns an instance of
the [RequestFile](./request_file) class:

```dart
RequestFile? file = $request->file('photo');
```

Or you can upload list of files with adding `'[]'` to your client upload field name `file[]` and retrieve list of
RequestFile:

```dart
List<RequestFile> file = $request->files('file');
```

Storing file in the storage directory

```dart
RequestFile? file = $request->file('photo');

if (file != null) {
  String path = file.store('app/image', 'filename.jpg');
}
```