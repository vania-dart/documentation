---
sidebar_label: 'WebScoket'
sidebar_position: 12
---

# Service Providers

## Introduction

WebSockets are a powerful tool for enabling real-time communication between clients in your application. With Vania, implementing WebSockets is straightforward and efficient. By enabling WebSockets in your `config/app.dart` file, you can easily set up real-time communication channels.

In your project's `Route` directory, you'll find a `web_socket.dart` file where you can define WebSocket routes.

## Quick Start

To get started with WebSockets in Vania, define your WebSocket routes using the router method. You can define different routes for distinct WebSocket functionalities:

### WebSocket Routes

```dart
// Default WebSocket route
Router.websocket('/ws', (WebSocketEvent event) {
    // Define your WebSocket events here
});

// WebSocket route for clients
Router.websocket('/clients', (WebSocketEvent event) {
    // Define WebSocket events for clients here
});

// WebSocket route for admins
Router.websocket('/admins', (WebSocketEvent event) {
    // Define WebSocket events for admins here
});
```

Within each route, you can define event listeners specific to the functionality of that route:

```dart
// Default WebSocket route
Router.websocket('/ws', (WebSocketEvent event) {
    event.on('message', (WebSocketClient client, dynamic message) {
        // Handle user-typing event for default route
    });
});

// WebSocket route for clients
Router.websocket('/clients', (WebSocketEvent event) {
    event.on('client-joined', (WebSocketClient client, dynamic message) {
        // Handle client-joined event for clients route
    });
});

// WebSocket route for admins
Router.websocket('/admins', (WebSocketEvent event) {
    event.on('admin-message', (WebSocketClient client, dynamic message) {
        // Handle admin-message event for admins route
    });
});
```

This organization allows you to manage WebSocket events efficiently based on different routes and functionalities.

Within each route, you can define event listeners:

```dart
Router.websocket('/ws', (WebSocketEvent event) {
    event.on('user-typing', (WebSocketClient client, dynamic message) {
        // Handle user-typing event
    });
});
```

:::info
You can also create WebSocket controllers and pass data to controller methods.
:::

Incoming data must adhere to the following JSON format:

```json
{
  "event": "message",
  "data": "client data"
}
```

:::info
The client data can be either a string or JSON format.
:::

## Client ID

Each user connected to the WebSocket server is assigned a client ID (session ID). To retrieve the client ID, you can use `client.clientId`.

```dart
client.clientId
```

## Responding to Clients

You can respond to clients using various methods:

### emit

```dart
client.emit(String event, dynamic payload);
```

To respond to the sender.

### toRoom

```dart
client.toRoom(String event, String room, dynamic payload)
```

To emit to all users in a specific room.

### to

```dart
client.to(String clientId, String event, dynamic payload)
```

To emit to a specific session ID.

### broadcast

```dart
client.broadcast(String event, dynamic payload)
```

To broadcast to all connected sessions except the sender.

## Room

To allow users to join or leave a room, you can send the following events:

```json
// To join a room
{
    "event":"joinRoom",
    "room":"unique room name or ID"
}

// To leave a room
{
    "event":"leftRoom",
    "room":"unique room name or ID"
}
```
