---
sidebar_label: 'Storage'
sidebar_position: 9
---

# Storage

## Introduction

You can use the Storage Vania class to store file in `storage/app/public` path insted of usinf Dart file class

## Configuration

The caching config is located in  `lib\config\app.dart`.

## Storage Usage

### Obtaining a Storage Instance

To obtain a Storage instance, you may use the Storage , which is what we will use throughout this documentation. The Storage provides convenient, terse access to the underlying implementations of the Vania Storage contracts:

```dart
class UserController extends Controller
{

    Future<Response> index() async
    {
        String? value = Storage.get('file name');
 
        return Response.json();
    }
}
```

## Retrieving Files

The `get` method may be used to retrieve the contents of a file.

```dart

Uint8List? value = Storage.getAsBytes('image.jps');

String? value = Storage.get('text.txt');

```

If the file you are retrieving contains Map, you may use the `map` method to retrieve the file and decode its contents:

```dart

Map<String,dynamic>? value = Storage.map('image.jps');

``

## Storing Files

The `put` method may be used to store file contents on a disk. The content can be list of ints or Strig

```dart

await Storage.put('image.jpg',content);

```

## Deleting Files

The `delete` method may be used to delete file from storage.

```dart

await Storage.delete('image.jpg');

```

## Exists Files

The `exists` method may be used to determine if a file exists on the disk:

```dart

await Storage.exists('image.jpg');

```

## File Metadata

In addition to reading and writing files, Vania can also provide information about the files themselves. For example, the size method may be used to get the size of a file in bytes:

```dart

int? size = await Storage.size('image.jpg');

```

The MIME type of a given file may be obtained via the mimeType method:

```dart

String? mimeType = await Storage.mimeType('image.jpg');

```
