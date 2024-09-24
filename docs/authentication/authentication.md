---
sidebar_label: 'Authentication'
sidebar_position: 1
---

# Authentication

## Introduction

Authentication is fundamental to web applications, ensuring secure access for users. In Vania, simplicity, security, and efficiency are paramount in implementing authentication features within your application.

Vania's authentication system revolves around two key components: guards and providers. These elements work harmoniously to authenticate users and regulate access to different parts of your application.

Guards act as the guardians of authentication, determining how users are verified for each request they make. Providers, on the other hand, furnish the necessary user information and authentication mechanisms to the guards, ensuring a robust authentication process.

Vania utilizes JSON Web Tokens (JWT) for authentication, offering a modern and secure approach to user validation

To configure authentication settings in your Vania application, you'll interact primarily with the `auth.php` configuration file located in the `config` directory. This centralized configuration hub empowers you to tailor authentication parameters to your application's unique needs.

Whether you're a seasoned developer or new to web development, Vania's authentication system is crafted to simplify the implementation process while upholding stringent security standards. With Vania, you can confidently deploy authentication features that meet user expectations and fortify your application against potential threats.

## Authentication Quickstart

First, prepare your configuration and specify your provider. The provider refers to the model that will supply user information during the authentication process.

```dart
Map<String, dynamic> authConfig = {
  'guards': {
    'default': {
      'provider': User(),
    },
  }
};
```

Next, create a migration for personal access tokens using the command below:

```shell
vania make:auth
```

After creating the Auth migration, run the `migrate` command to apply the changes to your database:

```shell
vania migrate
```

:::danger
Ensure that the database information is implemented in the `.env` file.
:::

This streamlined setup helps integrate authentication capabilities into your application, managing user access and security efficiently.

## Authenticating User and Obtaining Authentication Token

To authenticate a user in Vania, you'll first need to retrieve the user data from the model (provider) and then set it as the authenticated user using the `Auth` class. Here's how you can achieve this:

```dart
// Retrieve user data from the model (provider)
final Map<String, dynamic>? user = await User()
    .query()
    .where('email', '=', request.input('email'))
    .first();

// Check if user exists
if (user == null) {
  return Response.json(
    {'message': 'User not found'},
    404,
  );
}

// Check if the password matches
if (user['password'].toString() != request.input('password').toString()) {
  return Response.json(
    {'message': 'Wrong password'},
    401,
  );
}

// Authenticate the user and generate a token
Map token = Auth()
    .login(user)
    .createToken(expiresIn: Duration(hours: 24), withRefreshToken: true);

// Further actions after successful authentication
// For example, returning the token
return Response.json(token, 200);
```

If needed, you may specify an authentication guard before calling the login method:

```dart
Map token = Auth()
    .guard('admin')
    .login(user)
    .createToken(expiresIn: Duration(hours: 24), withRefreshToken: true);
```

:::info
If you want to get the refresh token you can set withRefreshToken `true`
:::

## Revoking Tokens

If you need to revoke access from users by deleting tokens, you can use the `deleteCurrentToken` and `deleteTokens` methods.

- The `deleteCurrentToken` method revokes the currently active token used by a session.
- The `deleteTokens` method revokes all tokens associated with the current user based on their user ID.

```dart
Auth().deleteCurrentToken();  // Deletes the current token
Auth().deleteTokens();        // Deletes all tokens for the current user
```

These methods are essential for managing user sessions and ensuring security, especially in scenarios where immediate token invalidation is required.

## Integrating with Third-Party Databases

If you're utilizing a third-party database as your primary database, you can configure the provider setting in the configuration file to `null`. This allows you to seamlessly integrate with external databases while still leveraging Vania's authentication system.

Once configured, you can utilize the `check` method of the Auth class. This method requires passing the user data as a map:

```dart
await Auth().check(token, userPayload);
```

## Protecting Routes

You can use route middleware to restrict access to authenticated users only. Here's an example of how to protect routes using middleware:

```dart
Router.get("/home", homeController.index)
      .middleware([AuthenticateMiddleware()]);
```

In this example, the `AuthenticateMiddleware` ensures that only authenticated users can access the `/home` route.

## Retrieving the Authenticated User

Once authentication is successfully completed, you can retrieve information about the authenticated user using simple methods provided by Vania. These methods allow you to access user details, such as their ID, name, or any other relevant information stored in the authentication system.

You can utilize these methods anywhere in your controller or closure:

```dart
// Retrieve the currently authenticated user
Auth().user()

// Retrieve the ID of the currently authenticated user
Auth().id()
```

If you specify an authentication guard, you can also use:

```dart
// Retrieve the currently authenticated user using a specific guard (e.g., 'admin')
Auth().guard('admin').user()
```
