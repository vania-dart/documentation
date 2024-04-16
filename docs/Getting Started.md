---
sidebar_label: Getting Started
sidebar_position: 2
---

# Quick Start 🚀

Ensure that you have the [Dart SDK](https://dart.dev) installed on your machine.

YouTube Video [Quick Start](https://www.youtube.com/watch?v=k8ol0F4bDKs)

[![Quick Start](http://img.youtube.com/vi/k8ol0F4bDKs/0.jpg)](https://www.youtube.com/watch?v=k8ol0F4bDKs "Quick Start")

:::info
Vania requires Dart `">=3.0.0 <4.0.0"`
:::

## Installing 🧑‍💻

```shell
# 📦 Install the vania cli from pub.dev
dart pub global activate vania_cli
```

## Creating a Project ✨

Use the `vania create` command to create a new project.

```shell
# 🚀 Create a new project called "blog"
vania create blog
```

## Start the Dev Server 🏁

Open the newly created project and start the development server.

```shell
# 🏁 Start the dev server
vania serve
```

:::tip
You can also include the --vm flag to enable VM service.
:::

## Create a Production Build 📦

Create a production build:

```shell
# 📦 Create a production build
vania build
```

:::caution
For production use, deploy using the provided `Dockerfile` and `docker-compose.yml` files to deploy anywhere.
:::

Example CRUD API Project [Github](https://github.com/vania-dart/example)
