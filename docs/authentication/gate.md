---
sidebar_label: 'Gate'
sidebar_position: 1
---

# Gate

## Gate Class

The `Gate` class in Vania allows you to manage and define authorization logic for your Dart application. It provides a mechanism to determine if a user is authorized to perform specific actions based on defined abilities. gates are defined within the boot method of the `app\providers\app_service_provider.dart` class

:::info

 If you don't already have the `app_service_provider.dart` class, you can create one using the CLI command `vania make:provider`

:::

## Methods

### 1. `define()`

The `define()` method allows you to define a new ability. Each ability is represented by a closure (function) that contains the logic to determine if a user can perform a specific action.

#### Usage:

```dart
Gate().define('canDeletePost', () {
  return user.id == post.userId;
});
```

- **Parameters**:
  - `ability` (String): The name of the ability.
  - `callback` (Function): A closure that contains the authorization logic. This function should return `true` if the user is authorized and `false` otherwise.

- **Example**:

```dart
Gate().define('canUpdatePost', () {
  return user.id == post.userId;
});
```

### 2. `allows()`

The `allows()` method is used to determine if the current user is allowed to perform a given action based on a defined ability.

#### Usage:

```dart
bool canDelete = Gate().allows('canDeletePost');
```

- **Parameters**:
  - `ability` (String): The name of the ability to check.
  
- **Returns**: `true` if the ability is allowed (the closure returns `true`), otherwise `false`.

- **Example**:

```dart
if (Gate().allows('canUpdatePost')) {
  // The user can update the post
} else {
  // The user is not authorized to update the post
}
```


### 3. `denies()`

The `denies()` method checks if the user is explicitly denied an action based on the ability. It returns `true` if the action is denied (i.e., if `allows()` returns `false`).

#### Usage:

```dart
bool cannotDelete = Gate().denies('canDeletePost');
```

- **Parameters**:
  - `ability` (String): The name of the ability to check.
  
- **Returns**: `true` if the ability is denied (the closure returns `false`), otherwise `false`.

- **Example**:

```dart
if (Gate().denies('canUpdatePost')) {
  // The user is not authorized to update the post
} else {
  // The user can update the post
}
```

### 4. `has()`

The `has()` method determines if a given ability has been defined. This is useful for checking whether a specific ability is available before attempting to authorize an action.

#### Usage:

```dart
bool hasAbility = Gate().has('canDeletePost');
```

- **Parameters**:
  - `ability` (String): The name of the ability to check.
  
- **Returns**: `true` if the ability is defined, otherwise `false`.

- **Example**:

```dart
if (Gate().has('canUpdatePost')) {
  print('The ability to update posts is defined.');
} else {
  print('The ability to update posts is not defined.');
}
```

### 5. `can()`

The `can()` method is a helper function that checks if an ability is allowed. It simplifies the usage of the `allows()` method.

#### Usage:

```dart
bool canUpdate = can('canUpdatePost');
```

- **Parameters**:
  - `ability` (String): The name of the ability to check.

- **Returns**: `true` if the ability is allowed (the closure returns `true`), otherwise `false`.

- **Example**:

```dart

if (can('canUpdatePost')) {
  print('User can update the post.');
}

```

### 6. `cannot()`

The `cannot()` method is a helper function that checks if an ability is denied. It simplifies the usage of the `denies()` method.

#### Usage:

```dart

bool cannotUpdate = cannot('canUpdatePost');

```

- **Parameters**:
  - `ability` (String): The name of the ability to check.

- **Returns**: `true` if the ability is denied (the closure returns `false`), otherwise `false`.

- **Example**:

```dart

if (cannot('canUpdatePost')) {
  print('User cannot update the post.');
}

```

## Example Usage

Here is an example of how to use the `Gate` class to define and check authorization logic:

```dart
void main() {
  var user = User(id: 1, isAdmin: false);
  var post = Post(id: 1, userId: 1);

  // Define a gate to check if the user can update the post
  Gate().define('canUpdatePost', () {
    return user.id == post.userId;
  });

  // Check if the user can update the post
  if (Gate().allows('canUpdatePost')) {
    print('User is authorized to update the post.');
  } else {
    print('User is not authorized to update the post.');
  }

  // Use helper methods for authorization checks
  if (can('canUpdatePost')) {
    print('User can update the post.');
  }

  if (cannot('canUpdatePost')) {
    print('User cannot update the post.');
  }

  // Check if the ability to update the post exists
  if (Gate().has('canUpdatePost')) {
    print('The update post ability is defined.');
  }
}
```
