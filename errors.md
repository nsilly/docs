# Error Handling

- [Error Handling](#error-handling)
    - [Introduction](#introduction)
    - [HTTP Exceptions](#http-exceptions)
    - [Response](#response)

<a name="introduction"></a>

## Introduction

When you start a new project, error and exception handling is already configured for you.

> {tip} Checkout code of [ExceptionHandler](https://github.com/nsilly/exceptions/blob/master/src/Exceptions/ExceptionHandler.js) on github

`ExceptionHandler` is registered to application in `app.js` that located in root directory

<a name="http-exceptions"></a>

## HTTP Exceptions

Some exceptions describe HTTP error codes from the server. For example, this may be a "page not found" error (404), an "unauthorized error" (401) or even a developer generated 500 error.

Checkout `ExceptionHandler` and https://github.com/nsilly/exceptions/blob/master/src/Exceptions/HttpStatusCode.js for more information

<a name="response"></a>

## Response

While an exception is occur the application return an http response with the code of that exception and given message.

Example

```javascript
publish() {
    throw new Exception('Method is not implemented', 1006);
}
```
