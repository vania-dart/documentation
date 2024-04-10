---
sidebar_label: 'Request File'
sidebar_position: 5
---

# Request File

## Introduction

Handling user-uploaded files, such as photos and documents, is a common requirement in web applications. The `RequestFile` class in Vania simplifies the process of storing these uploaded files.

## File Uploads

To store an uploaded file, use the `store` method, specifying the desired storage path:

```dart
class UserController extends Controller {
  /// Fetch user details
  Future<Response> index() async {
    Map<String, dynamic>? details =
        await User().query().where('id', '=', Auth().id()).first();

    return Response.json(details);
  }

  Future<Response> updateAvatar(Request request) async {
    request.validate({
      'avatar': 'file:jpg,jpeg,png',
    }, {
      'avatar.file': 'The avatar must be an image file.',
    });

    RequestFile? avatar = request.file('avatar');
    String avatarPath = '';

    if (avatar != null) {
      avatarPath = await avatar.store(
          path: 'users/${Auth().id()}',
          filename: avatar.getClientOriginalName); // The file path will be /storage/app/public/users/user_id/filename.jpg
    }

    await User().query().where('id', '=', Auth().id()).update({
      'avatar': avatarPath,
    });

    return Response.json({'message': 'User avatar updated successfully'});
  }
}
```

## Custom Path

If you need to store a file at a custom path, use the `move` method. This is useful for placing files in the `public` directory, allowing them to be accessed directly via URL (e.g., `https://mydomain.com/users/user_id/file.jpg`):

```dart
String path = await avatar.move('public/users/${Auth().id()}', avatar.getClientOriginalName);
```
