---
sidebar_label: 'Controller'
sidebar_position: 3
---

# Controller

## Introduction

Instead of defining all of your request handling logic as closures in your route files, you may wish to organize this behavior using "controller" classes. Controllers can group related request handling logic into a single class. For example, a UserController class might handle all incoming requests related to users, including showing, creating, updating, and deleting users. By default, controllers are stored in the app/http/controllers directory.

## Defining Controller

To quickly generate a new controller, you may run the make:controller Vania-cli command. By default, all of the controllers for your application are stored in the app/http/controllers directory

```shell
vania make:controller user_controller
```

## Basic Controller

Let's take a look at an example of a basic controller. A controller may have any number of public methods which will respond to incoming HTTP requests:

```dart
class UserController extends Controller
{
    Future show(int id)
    {
        var user = User().query().where('id','=',id).first();

        return  Response.json(user);
    }
}
```

Once you have written a controller class and method, you may define a route to the controller method like so:

```dart
Router.get("/user/{id}", const UserController().show);
```

When an incoming request matches the specified route URI, the show method on the app\http\controllers\user_controller class will be invoked and the route parameters will be passed to the method.

The response must be an object of Response class (json,html,file,download)
