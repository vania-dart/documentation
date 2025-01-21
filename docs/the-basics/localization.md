---
sidebar_label: 'Localization'
sidebar_position: 14
---

# Localization

## Introduction

Vania provides a powerful and easy-to-use localization feature that allows you to easily retrieve strings in different languages. This enables you to support multiple languages in your application, making it accessible to a wider audience.

## 1. Defining Translation Strings

### Using Short Keys

Translation strings are typically stored in files within the `lib/lang` directory of your project. Inside this directory, there will be a subdirectory for each language your application supports. Vania uses this structure to manage translation strings, including built-in strings for validation error messages.

Here's how the directory structure should look:

```shell
/lib/lang
    /en
        messages.json
    /es
        messages.json
```

Each language-specific folder contains a `messages.json` file, which holds the translation strings for that language. The contents of these files are key-value pairs where the key is a string identifier (e.g., `"welcome"`) and the value is the translated text in the respective language.

Example of a `messages.json` file in the `en` (English) folder:

```json
{
    "welcome": "Welcome to Vania!"
}
```

### Setting the Default Language

By default, the language of your application is set using the `APP_LOCALE` environment variable. You can change this value to specify which language should be used by default in your application.

For example, to set the default language to English, add the following line to your `.env` file:

```shell
APP_LOCALE=en
```
This sets the default language to English (`en`). The `APP_LOCALE` should match the language folder inside your `lang` directory (e.g., `en` for English).


If you want to use a custom language folder path, you can change the default path by setting the `LANG_PATH` variable in the `.env` file. For instance:

```shell
LANG_PATH=lang
```

This allows you to configure the localization folder to fit the structure of your application.

## 2. Retrieving Translation Strings

There are two simple ways to retrieve a translation string: using the `trans()` method or the `''.trans()` extension.

### Using the `trans()` Method

To retrieve a translated string, you can use the `trans()` method, passing the key for the translation as a string.

Example:

```dart
String message = trans('welcome');
```

### Using the `''.trans()` Extension

Alternatively, you can use the string extension method `.trans()` directly on a string.

Example:

```dart
String message = 'welcome'.trans();
```

Both methods will return the appropriate translation based on the current locale (default language or any other language you've set).

### Working with Placeholders in Translations

You can define placeholders in your translation strings to dynamically insert values into them. Placeholders are represented by curly braces `{}`. 

Example: A translation with a placeholder in `messages.json`:

```json
{
    "welcome": "Welcome, {name}!"
}
```

To replace the placeholders with actual values, pass a `Map<String, dynamic>` of replacements as the second argument to the `trans()` method or to the `.trans()` extension.

Example:

```dart
// Using trans() method
String message = trans('welcome', {'name': 'Vania'}); // Output: Welcome, Vania

// Using .trans() extension
String message = 'welcome'.trans({'name': 'Vania'}); // Output: Welcome, Vania
```

This way, you can easily replace the placeholders with dynamic content when retrieving translation strings.

## 3. Conclusion

With Vania's localization feature, managing multiple languages in your application is simple. By defining translation strings, setting up your language folders, and using the `trans()` method or `''.trans()` extension, you can easily support multiple languages and create a more inclusive experience for your users.
