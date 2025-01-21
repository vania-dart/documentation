---
sidebar_label: 'Vania Template'
sidebar_position: 15
---

# Vania Template

## Introduction

Vania Template is a simple, lightweight way to render views in a Your application. Template files use the `.html` extension and usually live in the `lib/views/template` folder.

You can return these templates from routes and controllers by using the `view` helper function. You can also pass data to a template by using the second argument of the `view` function:

```dart
Router.get('/dashboard', () => view('dashboard'));
```

If you want to pass data to the view, you can do:

```dart
Router.get('/dashboard', () => view('dashboard', {'name': 'Vania'}));
```

## Displaying Data

To show data in your templates, wrap your variables with `@{ }`. For example, if you pass a `name` variable to your template:

```dart
Router.get('/dashboard', () => view('dashboard', {'name': 'Vania'}));
```

You can display it in your `.html` file like this:

```html
Hello, @{ name }.
```

## If Statements

You can use if statements in your templates with these directives:  
- `{@ if condition @}`  
- `{@ elseif(condition) @}`  
- `{@ else @}`  
- `{@ endif @}`  

They work like normal if statements in other languages:

```html
{@ if is_admin @} 
    You are an admin!
{@ elseif(company) @} 
    You are a company user.
{@ else @} 
    You are a regular user.
{@ endif @}

{@ if (name == 'Vania') @} 
    Hi Vania!
{@ endif @}
```

## Switch Statements

Switch statements are also supported. Use `{@ switch @}`, then list your cases with `{@ case @}`, and close with `{@ endswitch @}`:

```html
{@ switch role @}
    {@ case admin @}
        <span style="color:red">Admin Role</span>
    {@ endcase @}

    {@ case user @}
        <span style="color:blue">User Role</span>
    {@ endcase @}

    {@ default @}
        <span style="color:gray">Default Role</span>
    {@ enddefault @}
{@ endswitch @}
```

## Loops

You can loop through lists or run standard `for` loops:

```html
{@ for user in users @}
    Index: @{ index } — Name: @{ user.name }
{@ endfor @}

{@ for i=0; i<3; i++ @}
    Index -> @{ i } — Name: @{ users[i].name }
{@ endfor @}

{@ for i=0; i<3; i++ @}
    {@ for j=0; j<=i; j++ @}
        @{ i } - @{ j }
    {@ endfor @}
{@ endfor @}
```

- In `{@ for user in users @}`, each `user` is taken from the `users` list, and `index` shows the loop number.  
- In `{@ for i=0; i<3; i++ @}`, this works like a normal for loop, counting from 0 to 2.

## Including Subviews

Use the `{@ include('path') @}` directive to include another template inside your current view. The included file will have access to the parent’s variables:

```html
<div>
    {@ include('shared/errors') @}
    <form>
        <!-- Form Contents -->
    </form>
</div>
```

You can also pass extra data to the included view:

```html
{@ include('view/name', {"title": "Vania Dart"}) @}
```

## Comments

To write comments that **won't** appear in the final HTML, wrap them with `{@# ... #@}`:

```html
{@# This comment will not appear in the rendered HTML #@}
```

## Layouts Using Template Inheritance

You can create layouts for your application by defining "sections" with the `{@ yield('sectionName') @}` directive:

```html
<html>
    <head>
        <title>App Name - {@ yield('title') @}</title>
    </head>
    <body>
        {@ yield('sidbar') @}
        <div class="container">
            {@ yield('content') @}
        </div>
    </body>
</html>
```

### Extending a Layout

In a child view, use the `{@ extends('path') @}` directive to inherit a layout. Then define sections with `{@ section('sectionName') @}`:

```html
{@ extends('partials/app') @}

{@ section('page-title', 'User Page') @}

{@ section('sidbar') @}
    This is the master sidebar.
{@ endsection @}

{@ section('content') @}
    <!-- Page content goes here -->
{@ endsection @}
```

Anything in `{@ section('sidbar') @}` or other sections will replace the `{@ yield('sidbar') @}` placeholder in the layout.

## Forms

### CSRF Field

When you create an HTML form, you should include a hidden CSRF token to protect against Cross-Site Request Forgery attacks. You can generate this token by using:

```html
<form method="POST" action="/profile">
    {@ csrf @}
    ...
</form>
```

If you need the raw CSRF token for an AJAX (JavaScript) request, you can use:

```html

<script>
   let token = "{@ csrf_token() @}";
   // Now you can include 'token' in your AJAX headers or data.
</script>

```

### Validation Errors

If your application uses validation and passes validation errors to the template (for example, in a variable called `errors`), you can check for them with `hasError('fieldName')` and display them with `{@ error('fieldName') @}`:

```html
<div class="fv-row mb-8">
    <input 
        type="email" 
        placeholder="Email" 
        name="email" 
        value="{@ old('email') @}" 
        class="form-control bg-transparent {@ if(hasError('email')) @} is-invalid {@ endif @}" 
    />
    {@ if(hasError('email')) @} 
        <div class="invalid-feedback">{@ error('email') @}</div> 
    {@ endif @}
</div>

<div class="fv-row mb-3">
    <input 
        type="password" 
        placeholder="Password" 
        name="password"
        class="form-control bg-transparent {@ if(hasError('password')) @} is-invalid {@ endif @}"
    />
    {@ if(hasError('password')) @} 
        <div class="invalid-feedback">{@ error('password') @}</div> 
    {@ endif @}
</div>
```

Here, `{@ old('email') @}` preserves the old input so the user doesn't have to retype it when the form is redisplayed.

## Showing Session Messages

Sometimes you might redirect back with an error or success message, for example `Response.back('error', 'Error Message')`. You can check if a session variable exists with `hasSession('key')`, and you can display it with `{@ session('key') @}`:

```html
{@ if(hasSession('error')) @}
    <div class="fv-plugins-message-container fv-plugins-message-container--enabled invalid-feedback">
        {@ session('error') @}
    </div> 
{@ endif @}
```
