---
sidebar_label: 'Middleware'
sidebar_position: 2
---

# Middleware

## Introduction

Middleware provide a convenient mechanism for inspecting and filtering HTTP requests entering your application. For example, Vania includes a middleware that verifies the user of your application is authenticated. If the user is not authenticated, the middleware will return unauthorized json response. However, if the user is authenticated, the middleware will allow the request to proceed further into the application.

Additional middleware can be written to perform a variety of tasks besides authentication. For example, a logging middleware might log all incoming requests to your application. There are several middleware included in the Vania framework, including middleware for authentication and CSRF protection.

You can create your custome middleware but it's must be in the app/http/middleware path and extends the base abstract Middleware class

## Defining Middleware

To create a new middleware, use the make:middleware Vania-cli command:

```shell
vania make:middleware custom_middleware
```

Simpale middleware class

```dart
class CustomMiddleware extends Middleware {
  @override
  handle(Request req) async {
    if(req.input('token') != 'My token'){
      throw HttpException(
        message: {'message':'Wrong token'},
        code: 422
      );
    }
    return next?.handle(req);
  }
}
```

As you can see, if the given token does not match our secret token, the middleware will return an HTTP exception to the client; otherwise, the request will be passed further into the application. To pass the request deeper into the application (allowing the middleware to "pass"), you should call the 'next?handle(req)' callback with the request.

## Assigning Middleware to Routes

If you would like to assign middleware to specific routes, you may invoke the middleware method when defining the route

```dart
Router.get("/User", (){}).middleware([CustomMiddleware()]);
```

Middleware method accept list of middlewares

```dart
Router.get("/User", (){}).middleware([AuthenticateMiddleware(),CustomMiddleware()]);
```

## Middleware Groups

Sometimes you may want to group several middleware under a single key to make them easier to assign to routes.

```dart
 Router.group([
    
    Router.get("/User", (){});
    Router.get("/profile", (){});

 ],middleware: [AuthenticateMiddleware(),CustomMiddleware()]);
```

## Throttle

To manage the number of requests a client can make to a particular endpoint within a set timeframe, Vania offers a `Throttle` middleware. This is particularly useful for preventing abuse and ensuring fair usage, especially when dealing with external APIs that have their own rate limits.

### Overview

The `Throttle` middleware limits the frequency of requests by allowing you to specify the maximum number of allowed attempts and the time period within which these attempts can occur. Once a client exceeds this limit, further requests are blocked until the time period elapses.

### Example Usage

Here's how you can apply the `Throttle` middleware to an endpoint in your Vania application. This example demonstrates limiting requests to three attempts per minute for a route that sends a one-time password (OTP) to a user:

```dart
Router.get("/get-otp", () async {
  // Your logic to send OTP
}).middleware([Throttle(maxAttempts: 3, duration: Duration(seconds: 60))]);
```

### Key Parameters

- **maxAttempts**: The maximum number of requests that a client can make to the endpoint during the specified duration.
- **duration**: The time period (in seconds) for which the rate limit is enforced.

### Implementing Throttle Middleware

To implement the `Throttle` middleware, you need to include it in the middleware array of your route definition. The middleware automatically tracks the number of requests from each client using their IP address and ensures that the request count does not exceed the specified threshold.

When the limit is reached, subsequent requests within the duration period are rejected, and the server can send a response indicating that the rate limit has been exceeded, which helps inform users or external systems to slow down requests.

This feature is critical for maintaining the integrity and availability of your services, especially when integrating with external systems that may penalize or block your access if too many requests are made in a short period.

By properly leveraging the `Throttle` middleware, you can control traffic flow to your application's endpoints, prevent abuse, and ensure compliance with external API rate limits.
