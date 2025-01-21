---
sidebar_label: 'Authentication'
sidebar_position: 1
---

# Authentication

## Introduction

Authentication is fundamental to web applications, ensuring secure access for users. In Vania, simplicity, security, and efficiency are prioritized when implementing authentication features.

Vania's authentication system revolves around two key components: **guards** and **providers**. Guards determine how users are authenticated for each request, while providers supply user information and authentication mechanisms. Vania uses **JSON Web Tokens (JWT)** for authentication, providing a secure approach to user validation.

To configure authentication in your Vania application, edit the `auth.php` file in the `config` directory. This file lets you define how authentication should work in your application.

Whether you're an experienced developer or new to web development, Vania's authentication system is designed to be straightforward while upholding high security standards.

## Authentication Quickstart

1. **Configure your provider**. The provider refers to the model that will supply user data during authentication:

   ```dart
   Map<String, dynamic> authConfig = {
     'guards': {
       'default': {
         'provider': User(),
       },
     }
   };
   ```

2. **Generate and run the auth migration**. This migration creates the necessary tables for storing personal access tokens:

   ```shell
   vania make:auth
   vania migrate
   ```

   > **Note**: Make sure your `.env` file has the correct database configuration before running the migration.

After these steps, your application is ready to handle basic authentication flows and manage user access.

## Authenticating a User & Getting a Token

To authenticate a user in Vania, retrieve their data from the provider (e.g., a `User` model) and then call the `Auth` class:

```dart
// Retrieve user data from the model (provider)
final Map<String, dynamic>? user = await User()
  .query()
  .where('email', '=', request.input('email'))
  .first();

// Check if user exists
if (user == null) {
  return Response.json({'message': 'User not found'}, 404);
}

// Check if the password matches
if (user['password'].toString() != request.input('password').toString()) {
  return Response.json({'message': 'Wrong password'}, 401);
}

// Authenticate the user and generate a token
Map token = Auth()
  .login(user)
  .createToken(
    expiresIn: Duration(hours: 24),
    withRefreshToken: true,
  );

// Return the token (for example)
return Response.json(token, 200);
```

### Using a Specific Guard

If your app uses multiple guards (like `admin`), specify it before calling `login`:

```dart
Map token = Auth()
  .guard('admin')
  .login(user)
  .createToken(expiresIn: Duration(hours: 24), withRefreshToken: true);
```

## Basic Authentication

In some situations, you may need to use **Basic Auth** instead of or in addition to JWT. You can pass `true` as the second argument to `login` for a basic authentication flow:

```dart
// Basic authentication
Auth().login(user, true);
```

### Basic Auth Middleware

You can create an authentication middleware that supports Basic Auth by extending the `Authenticate` class and enabling `basic: true`:

```dart
class AuthenticateMiddleware extends Authenticate {
  AuthenticateMiddleware({
    super.guard,
    super.basic,
    super.loginPath,
  });
}
```

Then, apply it to your routes:

```dart
Router.delete('/home', databaseController.destroy)
  .middleware([AuthenticateMiddleware(basic: true)]);
```

With `basic: true`, the `AuthenticateMiddleware` will expect Basic Authentication credentials.

## Revoking Tokens

If you need to revoke a token, you can use:

```dart
// Delete only the current token
Auth().deleteCurrentToken();

// Delete all tokens for the current user
Auth().deleteTokens();
```

These methods are crucial for managing user sessions, allowing you to immediately invalidate tokens if necessary.

## Integrating with Third-Party Databases

If you use a third-party database and don’t want to rely on a local provider, set the provider to `null` in your config. Then call:

```dart
await Auth().check(token, userPayload);
```

Here, `token` is the JWT or relevant credential, and `userPayload` is a map of user data from your external database or service.

## Protecting Routes

Use route middleware to ensure only authenticated users can access certain endpoints:

```dart
Router.get("/home", homeController.index)
      .middleware([AuthenticateMiddleware()]);
```

Here, `AuthenticateMiddleware` checks if the user is authenticated before allowing access to the `/home` route.

## Getting the Authenticated User

After a user is authenticated, you can retrieve their details with:

```dart
// Full user data
Auth().user();

// Only the user ID
Auth().id();
```

Or, if you use a different guard:

```dart
Auth().guard('admin').user();
```