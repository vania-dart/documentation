---
id: validation
title: Validation
sidebar_position: 7
---

## Introduction

Vania offers various approaches to validate incoming data in your application. While the most common method is using
the `validate` method on HTTP requests, Vania supports other validation techniques as well.

With a comprehensive set of validation rules, Vania enables you to validate data conveniently, This guide will cover
each validation rule extensively, ensuring you are well-acquainted with Vania's validation capabilities.

## Simple Validation Example

```dart
Future<Response> index(Request req) async {
  req.validate({ 
    'name': 'required|string|alpha',
    'email': 'required|email'
  });
}
```

## Validation with Custom Messages

```dart
Future<Response> index(Request req) async {
  req.validate(
    { 
      'email': 'required|email',
      'name': 'required|string|alpha'
    },
    {
      'name.required': 'Name is required', 
      'email.required': 'Email is required',
      'email.email': 'Invalid email format',
      'name.string': 'Name must be a string', 
      'name.alpha': 'Name must contain only alphabetic characters'
    }
  );
}
```

## Nested Validation

### Nested Validation Example with JSON Data

Validation

```dart
req.validate({
  'address.name': 'required',
  'address.type': 'required|numeric'
});
```

Request body

```json
{
  "address": {
    "name": "Vania",
    "type": "Framework"
  }
}
```

### Nested Validation with Array

Validation

```dart
req.validate({
  'products.*.item': 'required',
  'products.*.options.*.price': 'required'
});
```

Request body

```json
{
  "products": [
    {
      "item": "Item 1",
      "options": [
        {
          "price": "22000",
          "color": "blue"
        }
      ]
    },
    {
      "item": "Item 2",
      "options": [
        {
          "price": "38000",
          "color": "blue"
        }
      ]
    }
  ]
}
```

:::tip
Validation rules are applicable within route closures or controller methods, providing flexibility and ease of use.
:::

## Rules

**required**: Ensures that the field is present and not empty.
- Example: `'name': 'required'`

```dart
req.validate({'name': 'required'});
```

**required_if**: Requires the field to be present if another field has a specific value.
- Example: `'name': 'required_if:status,active'`

```dart
req.validate({
  'name': 'required_if_not:status,active',
  'status': 'in:active,inactive'
});
```

:::info
Name is required if status is inactive.
:::

**email**: Validates that the field is a valid email address.
- Example: `'email': 'email'`

```dart
req.validate({'email': 'email'});
```

**string**: Validates that the field is a string.
- Example: `'name': 'string'`

```dart
req.validate({'name': 'string'});
```

**numeric**: Validates that the field contains only numeric characters.
- Example: `'age': 'numeric'`

```dart
req.validate({'age': 'numeric'});
```

**boolean**: Validates that the field is a boolean value (`true` or `false`).
- Example: `'active': 'boolean'`

```dart
req.validate({'active': 'boolean'});
```

**integer**: Validates that the field is an integer number.
- Example: `'quantity': 'integer'`

```dart
req.validate({'quantity': 'integer'});
```

**array**: Validates that the field is an array.
- Example: `'items': 'array'`

```dart
req.validate({'items': 'array'});
```

**json**: Validates that the field is a valid JSON string.
- Example: `'data': 'json'`

```dart
req.validate({'data': 'json'});
```

**ip**: Validates that the field is a valid IP address.
- Example: `'ip_address': 'ip'`

```dart
req.validate({'ip_address': 'ip'});
```

**alpha**: Validates that the field contains only alphabetic characters.
- Example: `'name': 'alpha'`

```dart
req.validate({'name': 'alpha'});
```

**alpha_dash**: Validates that the field contains only alphanumeric characters, dashes, and underscores.
- Example: `'username': 'alpha_dash'`

```dart
req.validate({'username': 'alpha_dash'});
```

**alpha_numeric**: Validates that the field contains only alphanumeric characters.
- Example: `'password': 'alpha_numeric'`

```dart
req.validate({'password': 'alpha_numeric'});
```

**date**: Validates that the field is a valid date format.
- Example: `'dob': 'date'`

```dart
req.validate({'dob': 'date'});
```

**url**: Validates that the field is a valid URL.
- Example: `'website': 'url'`

```dart
req.validate({'website': 'url'});
```

**uuid**: Validates that the field is a valid UUID.
- Example: `'user_id': 'uuid'`

```dart
req.validate({'user_id': 'uuid'});
```

**in**: Validates that the field's value is in a predefined list of values.
- Example: `'status': 'in:active,inactive'`

```dart
req.validate({'status': 'in:active,inactive'});
```

**not_in**: Validates that the field's value is not in a predefined list of values.
- Example: `'role': 'not_in:admin,superuser'`

```dart
req.validate({'role': 'not_in:admin,superuser'});
```

**start_with**: Validates that the field's value starts with a specific string.
- Example: `'title': 'start_with:vania'`

```dart
req.validate({'title': 'start_with:vania'});
```

**end_with**: Validates that the field's value ends with a specific string.
- Example: `'title': 'end_with:framework'`

```dart
req.validate({'title': 'end_with:framework'});
```

**confirmed**: Validates that the field matches another field's value (usually used for password confirmation).
- Example: `'password': 'confirmed'`

```dart
req.validate({'password': 'confirmed'});
```

**file**: Validates that the field is a file upload.
- Example: `'avatar': 'file'`

```dart
req.validate({'avatar': 'file'});
```

**min_length**: Validates that the field's length is at least a minimum value.
- Example: `'password': 'min_length:8'`

```dart
req.validate({'password': 'min_length:8'});
```

**max_length**: Validates that the field's length does not exceed a maximum value.
- Example: `'username': 'max_length:20'`

```dart
req.validate({'username': 'max_length:20'});
```

**length_between**: Validates that the field's length is between a minimum and maximum value.
- Example: `'name': 'length_between:3,50'`

```dart
req.validate({'name': 'length_between:3,50'});
```

**min**: Validates that the field's value is at least a minimum numeric value.
- Example: `'age': 'min:18'`

```dart
req.validate({'age': 'min:18'});
```

**max**: Validates that the field's value does not exceed a maximum numeric value.
- Example: `'price': 'max:1000'`

```dart
req.validate({'price': 'max:1000'});
```

**between**: Validates that the field's value is between a minimum and maximum numeric value.
- Example: `'rating': 'between:1,5'`

```dart
req.validate({'rating': 'between:1,5'});
```

**greater_than**: Validates that the field's value is greater than a specified numeric value.
- Example: `'quantity': 'greater_than:0'`

```dart
req.validate({'quantity': 'greater_than:0'});
```

**less_than**: Validates that the field's value is less than a specified numeric value.
- Example: `'age': 'less_than:100'`

```dart
req.validate({'age': 'less_than:100'});
```