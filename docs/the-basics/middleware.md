---
id: middleware
title: Middleware
sidebar_position: 2
---

## Introduction

Middleware provide a convenient mechanism for inspecting and filtering HTTP requests entering your application. For
example, Vania includes a middleware that verifies the user of your application is authenticated. If the user is not
authenticated, the middleware will return unauthorized json response. However, if the user is authenticated, the
middleware will allow the request to proceed further into the application.

Additional middleware can be written to perform a variety of tasks besides authentication. For example, a logging
middleware might log all incoming requests to your application. There are several middleware included in the Vania
framework, including middleware for authentication and CSRF protection.

You can create your custom middleware, but it must be in the app/http/middleware path and extends the base abstract
Middleware class

## Defining Middleware

To create a new middleware, use the make:middleware Vania-cli command:

```shell
vania make:middleware custom_middleware
```

Simple middleware class

```dart
class CustomMiddleware extends Middleware {
  @override
  handle(Request req) async {
    if(req.input('token') != 'My token'){
      throw HttpException(
        message: {'message': 'Wrong token'},
        code: 422
      );
    }
    next?.handle(req);
  }
}
```

As you can see, if the given token does not match our secret token, the middleware will return an HTTP exception to the
client; otherwise, the request will be passed further into the application. To pass the request deeper into the
application (allowing the middleware to `pass`), you should call the 'next?handle(req)' callback with the request.

## Assigning Middleware to Routes

If you would like to assign middleware to specific routes, you may invoke the middleware method when defining the route

```dart
Router.get("/User", (){})
      .middleware([CustomMiddleware()]);
```

Middleware method accept list of middlewares

```dart
Router.get("/User", (){})
      .middleware([AuthenticateMiddleware(), CustomMiddleware()]);
```

## Middleware Groups

Sometimes you may want to group several middleware under a single key to make them easier to assign to routes.

```dart
Router.group([
    
  Router.get("/User", (){});
  
  Router.get("/profile", (){});

], middleware: [AuthenticateMiddleware(), CustomMiddleware()]);
```