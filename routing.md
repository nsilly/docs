# Routing

- [Routing](#routing)
    - [Basic Routing](#basic-routing)
    - [The Default Route Files](#the-default-route-files)
    - [Available Router Methods](#available-router-methods)
    - [Route paths](#route-paths)
    - [Middleware](#middleware)

<a name="basic-routing"></a>

## Basic Routing

nsilly is built on top of [Express](https://expressjs.com), it providing a very simple and expressive method of defining routes:

> {tip} Checkout offical documentation [https://expressjs.com/en/guide/routing.html](https://expressjs.com/en/guide/routing.html)

```
var express = require('express')
var app = express()

// respond with "hello world" when a GET request is made to the homepage
app.get('/', function (req, res) {
  res.send('hello world');
})
```

<a name="the-default-route-files"></a>

## The Default Route Files

All routes are located in the `routes` directory.

For most applications, you will begin by defining routes in your `routes` directory and load it to `routes/index.js`.

```javascript
import express from "express";
import bcrypt from "bcrypt-nodejs";
import UserTransformer from "../../../app/Transformers/UserTransformer";
import UserRepository from "../../../app/Repositories/UserRepository";
import { App } from "@nsilly/container";
import { Request, AsyncMiddleware } from "@nsilly/support";
import { ApiResponse } from "@nsilly/response";

var router = express.Router();

async function index(req, res) {
  const users = await App.make(UserRepository).get();
  res.json(ApiResponse.collection(users, new UserTransformer()));
}

async function store(req, res) {
  const data = {
    email: Request.get("email"),
    password: bcrypt.hashSync(
      Request.get("password"),
      bcrypt.genSaltSync(8),
      null
    )
  };
  const user = await App.make(UserRepository).create(data);
  res.json(ApiResponse.item(user, new UserTransformer()));
}

router.get("/", AsyncMiddleware(index));
router.post("/", AsyncMiddleware(store));

export default router;
```

<a name="available-router-methods"></a>

## Available Router Methods

The router allows you to register routes that respond to any HTTP verb:

```javascript
router.get(uri, handler);
router.post(uri, handler);
router.put(uri, handler);
router.patch(uri, handler);
router.delete(uri, handler);
router.options(uri, handler);
```

<a name="route-paths"></a>

## Route paths

Route paths, in combination with a request method, define the endpoints at which requests can be made. Route paths can be strings, string patterns, or regular expressions.

The characters ?, +, \*, and () are subsets of their regular expression counterparts. The hyphen (-) and the dot (.) are interpreted literally by string-based paths.

If you need to use the dollar character ($) in a path string, enclose it escaped within ([ and ]). For example, the path string for requests at “/data/$book”, would be “/data/([\$])book”.

Here are some examples of route paths based on strings.

This route path will match requests to the root route, /.

```javascript
app.get("/", function(req, res) {
  res.send("root");
});
```

This route path will match requests to /about.

```javascript
app.get("/about", function(req, res) {
  res.send("about");
});
```

This route path will match requests to /random.text.

```javascript
app.get("/random.text", function(req, res) {
  res.send("random.text");
});
```

Here are some examples of route paths based on string patterns.

This route path will match acd and abcd.

```javascript
app.get("/ab?cd", function(req, res) {
  res.send("ab?cd");
});
```

This route path will match abcd, abbcd, abbbcd, and so on.

```javascript
app.get("/ab+cd", function(req, res) {
  res.send("ab+cd");
});
```

This route path will match abcd, abxcd, abRANDOMcd, ab123cd, and so on.

```javascript
app.get("/ab*cd", function(req, res) {
  res.send("ab*cd");
});
```

This route path will match butterfly and dragonfly, but not butterflyman, dragonflyman, and so on.

```javascript
app.get(/.*fly$/, function(req, res) {
  res.send("/.*fly$/");
});
```

<a name="middleware"></a>

## Middleware

To assign middleware to all routes within a group, you may use `*` willcard. Middleware are executed in the order they are listed:

> {tip} Checkout offical documentation for more information [https://expressjs.com/en/guide/using-middleware.html](https://expressjs.com/en/guide/using-middleware.html)

```javascript
router.get("/", AsyncMiddleware(index)); // AuthMiddleware is not applied
router.all("*", AuthMiddleware);
router.post("/", AsyncMiddleware(store)); // AuthMiddleware is applied
```
