---
id: providers
title: Service Providers
sidebar_position: 11
---

## Introduction

One of the powerful features of Vania is its Service Provider architecture. Service Providers act as the backbone of the
framework, initializing essential components before anything else. Each Service Provider in Vania implements two crucial
methods: `register` and `boot`.

When your application starts up, Vania executes the `register` method of each Service Provider first, followed by
the `boot` method. This sequential execution ensures that necessary services are registered and configured before they
are utilized within the application.

To leverage Service Providers effectively, you create custom providers that extend the base `ServiceProvider` class
provided by Vania. These custom providers encapsulate any third-party integrations or initialization routines specific
to your application.

When you generate a new Service Provider using the Vania CLI, a corresponding file is created in the `app/providers`
directory. To activate the newly created provider, you simply include it in the list of providers within
the `config/app.config` file.

For example, to generate a new provider named `my_new_provider`, you can use the following command:

```shell
vania make:provider my_new_provider
```

Below is a basic example of a Service Provider in Vania:

```dart
class MyNewProvider extends ServiceProvider {
  @override
  Future<void> boot() async {
    // Perform any initialization tasks after registration
  }

  @override
  Future<void> register() async {
    // Register services or perform setup tasks here
    // For example, initializing a NoSQL database like Hive
  }
}
```

In this example, the `register` method is used for registering services or performing setup tasks, such as initializing
a NoSQL database like Hive. The `boot` method, on the other hand, can be utilized for additional initialization tasks
after service registration.