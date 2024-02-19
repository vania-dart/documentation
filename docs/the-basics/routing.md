---
sidebar_label: 'Routeing'
sidebar_position: 1
---


# Routeing

## The Default Route Files And Route Class

All Vania routes are defined in your route files, which are located in the routes directory. These files are automatically loaded by your application's ***App\Providers\RouteServiceProvider***. The routes/api.dart is for Http api requests. and The routes/websockets.dart is for WebSockets.

The route file must implement the Route abstract class and override the register method, all routes must defined in this method

```dart

import 'package:vania/vania.dart';

class ApiRoute implements Route{
  @override
  void register() {
    Router().basePrefix('api');
   // Routers
  }
}
```

For most applications, you will begin by defining routes in your routes/api.dart file. The routes defined in routes/api.dartv may be accessed by entering the defined route's URL in your browser. For example, you may access the following route by navigating to [http://example.com/api/hello-world] in your browser:

```dart
Router.get('/hello-world', () {
    return Response.html('<h1>Hello, World!</h1>');
});
```

## Basic Routeing

The most basic Vania routes accept a URI and a closure, providing a very simple and expressive method of defining routes and behavior without complicated

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

Sometimes you will need to capture segments of the URI within your route. For example, you may need to capture a user's ID from the URL. You may do so by defining route parameters:

```dart
Router.get('/user/{id}', (int id) {
  return Response.html('User id $id');
});
```

You may define as many route parameters as required by your route:

```dart
Router.get('/posts/{post}/comments/{comment}', function (string postId, string commentId) {
    
});
```

Route parameters are always encased within {} braces and should consist of alphabetic characters. Underscores (_) are also acceptable within route parameter names. Route parameters are injected into route callbacks / controllers based on their order - the names of the route callback / controller arguments do not matter.

## Route Groups

Route groups allow you to share route attributes, such as middleware and prefix across a large number of routes without needing to define those attributes on each individual route.

```dart

Router.group([
  Router.get('/', () {}), // https://mysite.com/api/v1/users
  Router.get('/details/{id}', (int id) {}), // https://mysite.com/api/v1/users/details/1
  Router.post('create',(Request request){}), // https://mysite.com/api/v1/users/create
],prefix: 'v1/users');

```

## Route Prefix

The prefix method may be used to prefix each route in the group with a given URI. For example, you may want to prefix all route URIs within the group with users

You can use prefixes for single or group router

```dart
 // https://mysite.com/api/users/details/1

Router.get('/details/{id}', (int id) {}).prefix('users');

```

## Websocket

If you want to use WebSocket in your project you can define the WebSocket route, After creating the WebSocket router you need to define WebSocket listeners

With the WebSocket listeners, you can listen to client events

```dart
class WebSocketRoute implements Route{
  @override
  void registery() {

    Router.websocket('/ws',(WebSocketEvent event){

      event.on('message', (WebSocketClient client,dynamic message){

        print(message);

      });

      event.on('sendMessage', (WebSocketClient client,dynamic message){

        client.toRoom('message',"MyRoom",message);
        
      });

    });
  }

}
```
