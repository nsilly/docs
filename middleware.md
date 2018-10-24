# Middleware

- [Middleware](#middleware)
    - [Introduction](#introduction)
    - [Built-in Middleware](#built-in-middleware)
        - [AsyncMiddleware](#asyncmiddleware)

<a name="introduction"></a>

## Introduction

Middleware provide a convenient mechanism for filtering HTTP requests entering your application. For example, a middleware that verifies the user of your application is authenticated. If the user is not authenticated, the middleware will redirect the user to the login screen or throw an exception. However, if the user is authenticated, the middleware will allow the request to proceed further into the application.

Of course, additional middleware can be written to perform a variety of tasks besides authentication. A logging middleware might log all incoming requests to your application.

There are several middleware included in the framework, including middleware for authentication and Async Middleware. All of these middleware are located in the `app/Middlewares` directory.

<a name="defining-middleware"></a>

## Built-in Middleware

> {tip} Writing middleware for use in Express apps https://expressjs.com/en/guide/writing-middleware.html

### AsyncMiddleware

Allow us use Async/Await in your handler

```javascript
async function index(req, res) {
  const users = await App.make(UserRepository).get();
  res.json(ApiResponse.collection(users, new UserTransformer()));
}

router.get("/", AsyncMiddleware(index));
```
