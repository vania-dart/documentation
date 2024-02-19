---
sidebar_label: 'Responses'
sidebar_position: 5
---

# Responses

All routes and controllers should return a Response class to be sent back to the user's. Vania provides several different ways to return responses:

The json method will automatically set the Content-Type header to application/json, as well as convert the given list/map to JSON:

```dart
Route.get('/', (Request request) {
    return Response.json({});
});
```

The html method will automatically set the Content-Type header to text/html; charset=utf-8, as well as convert the given string/html tag to HTML:

```dart
Route.get('/', (Request request) {
    return Response.html('<h1>Html Tag</h1>);
});
```

The download method may be used to generate a response that forces the user's to download the file at the given path. The download method accepts the absolute path to the file as its first argument:

```dart
Route.get('/', (Request request) {
    return Response.download('file path for download file);
});
```

The file method may be used to display a file, such as an image or PDF, directly in the user's browser instead of initiating a download. This method accepts the absolute path to the file as its first argument:

```dart
Route.get('/', (Request request) {
    return Response.file('file path for stream file);
});
```
