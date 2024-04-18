---
sidebar_label: 'Routing'
sidebar_position: 1
---


# Routing

## Introduction

In Vania, routes are the pathways that direct incoming requests from the client's browser to specific handlers or controllers within your application. These routes are defined in dedicated route files located in the `routes` directory of your project.

Your application automatically loads these route files through the `app/providers/route_service_provider`, which helps manage and organize your routes effectively. Within the `routes` directory, you'll typically find files like `api_route.dart` and `websockets.dart`, each serving specific purposes.

For instance, the `api_route.dart` file is responsible for handling HTTP API requests, while `websockets.dart` manages WebSocket connections.

To define routes effectively, each route file must implement the Route abstract class and override the `register` method. This method acts as the central hub where all routes are declared, directing requests to their respective controllers or handlers.

By adhering to this structure, Vania ensures a clear and organized routing system, allowing developers to efficiently manage and expand their application's endpoints while maintaining readability and scalability.

```dart

import 'package:vania/vania.dart';

class ApiRoute implements Route{
  @override
  void register() {
    Router.basePrefix('api');
   // Routers
  }
}
```

For most applications, you will begin by defining routes in your `routes/api_route.dart` file. The routes defined in `routes/api_route.dart` may be accessed by entering the defined route's URL in your browser. For example, you may access the following route by navigating to [http://example.com/api/hello-world] in your browser:

```dart
Router.get('/hello-world', () {
    return Response.html('<h1>Hello, World!</h1>');
});
```

:::caution
The `basePrefix` method in Vania's router allows you to set a base prefix for all routes defined within its scope. When applied, this prefix will be automatically prepended to the beginning of each route URI within the router, simplifying route management and organization.

For example, if you set the base prefix to `'api'`, all routes defined within the router will have `'api'` prefixed to their URIs. This feature is particularly useful when you have a set of routes that share a common base path, such as API endpoints.

Usage:

```dart

Router.basePrefix('api');

```

With this base prefix set, any routes registered within the router will automatically include `'api'` at the beginning of their URIs. This helps maintain consistency and clarity in your application's routing structure.
:::

## Basic Routing

Vania's basic routes accept a URI and a closure, providing a simple and expressive method for defining routes and behavior without complexity.

```dart
Router.get('/greeting', () {
    return Response.html('<h1>Hello, World!</h1>');
});
```

## Available Router Methods

The router allows you to register routes that respond to any HTTP verb:

```dart
Router.get(uri, callback);
Router.post(uri, callback);
Router.put(uri, callback);
Router.patch(uri, callback);
Router.delete(uri, callback);
Router.options(uri, callback);
Router.websocket(uri, callback);
```

## Route Parameters

Sometimes, you'll need to capture segments of the URI within your route. For example, you may need to capture a user's ID from the URL. You can do so by defining route parameters:

```dart
Router.get('/user/{id}', (int id) {
  return Response.html('User id $id');
});
```

You can define as many route parameters as required by your route:

```dart
Router.get('/posts/{post}/comments/{comment}', (String postId, String commentId) {
    
});
```

Route parameters are always enclosed within {} braces and should consist of alphabetic characters. Underscores (_) are also acceptable within route parameter names. Route parameters are injected into route callbacks/controllers based on their order; the names of the route callback/controller arguments do not matter.

## Route Groups

Route groups allow you to share route attributes, such as middleware and prefix, across a large number of routes without needing to define those attributes on each individual route.

```dart
Router.group((){
  Router.get('/', () {}), // https://mysite.com/api/v1/users
  Router.get('/details/{id}', (int id) {}), // https://mysite.com/api/v1/users/details/1
  Router.post('/create',(Request request){}), // https://mysite.com/api/v1/users/create
}, prefix: 'v1/users', middleware: [Authenticate()]);
```

## Route Prefix

The prefix method may be used to prefix each route in the group with a given URI. For example, you may want to prefix all route URIs within the group with 'users'.

You can use prefixes for single or group router.

```dart
// https://mysite.com/api/users/details/1

Router.get('/details/{id}', (int id) {}).prefix('users');
```

## WebSocket

If you want to use WebSockets in your project, you can define WebSocket routes. After creating the WebSocket router, you need to define WebSocket listeners.

With WebSocket listeners, you can listen to client events.

```dart
class WebSocketRoute implements Route {
  @override
  void register() {
    Router.websocket('/ws', (WebSocketEvent event) {
      event.on('message', (WebSocketClient client, dynamic message) {
        print(message);
      });

      event.on('sendMessage', (WebSocketClient client, dynamic message) {
        client.toRoom('message', "MyRoom", message);
      });
    });
  }
}
```

This revision clarifies the usage of routes in Vania and improves readability
