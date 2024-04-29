---
sidebar_label: 'Globe'
sidebar_position: 2
---

# Deploying Vania Using Globe

[Globe](https://globe.dev/) is a deployment platform tailored for Dart developers. It enables you to deploy your Vania backend projects to a globally distributed service without the need for server management.

## Deployment Steps

Before beginning, ensure you have a [Globe account](https://globe.dev/login). If you do not, you can sign up for a free account.

Next, install the [Globe command line interface (CLI)](https://docs.globe.dev/cli#installation) on your computer.

### Step-by-Step Guide

**Log into Globe**:
   Start by logging into Globe using the following command:

   ```shell
   globe login
   ```

:::info
Ensure that the `host` setting in your .env file is configured to `0.0.0.0`.
:::

**Deploy Your API for Production**:
   Use the command below to deploy your application for production use:

   ```shell
   globe deploy --prod
   ```

Specify the Dart entrypoint file: `bin/server.dart`

```shell
Enter a Dart entrypoint file: (lib/main.dart) `bin/server.dart`
```

Select the option to upload your local .env file:

```shell
An .env file was detected, would you like to:

Upload local environment variables

```

:::info
Globe is currently in public preview.
:::

:::warning
Globe does not support databases directly. You will need to set up your database server separately with another service provider.
:::

## Additional Resources

- [Globe Documentation](https://docs.globe.dev/)
- [`globe deploy` Documentation](https://docs.globe.dev/cli/commands/deploy)
- [Globe's Dart Frog Documentation](https://docs.globe.dev/frameworks/dart-frog)

These resources provide further guidance on deploying applications with Globe and utilizing Dart-specific frameworks.