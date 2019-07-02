# Responses

- [Responses](#responses)
    - [Creating Responses](#creating-responses)
            - [Strings & Arrays](#strings--arrays)
            - [Response Objects](#response-objects)
    - [Transformer](#transformer)
        - [Create a transformer](#create-a-transformer)
        - [Transform an object](#transform-an-object)
        - [Including Data](#including-data)

> {tip} Documentation for express app response https://expressjs.com/en/4x/api.html#res

<a name="creating-responses"></a>

## Creating Responses

<a name="strings--arrays"></a>

#### Strings & Arrays

All routes should return a response to be sent back to the user's browser. nsilly provides several different ways to return responses. The most basic response is returning a string from a route. The framework will automatically convert the string into a full HTTP response:

```javascript
app.get("/string", function(req, res) {
  res.send("A very long text");
});
```

In addition to returning strings from your routes, you may also return arrays. The framework will automatically convert the array into a JSON response:

```javascript
app.get("/array", function(req, res) {
  res.json(["an", "array", "of", "items"]);
});
```

<a name="response-objects"></a>

#### Response Objects

Typically, you won't just be returning simple strings or arrays from your route actions. Instead, you will be returning full `Response` with defined structure for your API.

nsilly has built-in `Response` class that is a wrapper and renderer for objects

> {tip} What is [Transformer](#transformer)?

An example of response that return a collection of user or a single user object

```javascript
import express from "express";
import UserTransformer from "../../../app/Transformer/UserTransformer";
import UserRepository from "../../../app/Repositories/UserRepository";
import { App } from "@nsilly/container";
import { AsyncMiddleware } from "@nsilly/support";
import { ApiResponse } from "@nsilly/response";

var router = express.Router();

async function index(req, res) {
  const users = await App.make(UserRepository).get();
  res.json(ApiResponse.collection(users, new UserTransformer()));
}

async function show(req, res) {
  const user = await App.make(UserRepository).findById(req.params.id);
  res.json(ApiResponse.item(user, new UserTransformer()));
}

router.get("/", AsyncMiddleware(index));
router.get("/:id", AsyncMiddleware(show));

export default router;
```

Return a collection of object

```javascript
res.json(ApiResponse.collection(collection, new ObjectTransformer()));
```

Return a single object

```javascript
res.json(ApiResponse.item(object, new ObjectTransformer()));
```

Result with pagination

```javascript
const items = App.make(ObjectRepository).paginate();

res.json(ApiResponse.paginate(items, new ObjectTransformer()));
```

Result with a success message

```javascript
res.json(ApiResponse.success());
```

Result with an array

```javascript
res.json(ApiResponse.array(["an", "array", "of", "items"]));
```

<a name="transformer"></a>

## Transformer

Transformer provides a presentation and transformation layer for complex data output, the like found in RESTful APIs, and works really well with JSON

- Create a “barrier” between source data and output, so schema changes do not affect users

- Systematic type-casting of data, to avoid foreach()ing through and (bool)ing everything

- Include (a.k.a embedding, nesting or side-loading) relationships for complex data structures

- Support the pagination of data results, for small and large data sets alike

- Generally ease the subtle complexities of outputting data in a non-trivial API

<a name="create-a-transformer"></a>

### Create a transformer

All transformer are located in `app/Transformers` directory. You can create new one by yourself or it will be created automatically by run following command:

```
yarn command make:transformer {name}
```

Example

```
yarn command make:transformer UserTransformer
```

<a name="transform-an-object"></a>

### Transform an object

By return an object via `transform` function we can transform the object that passed to `Response`

```javascript
import Transformer from "./Transformer";

export default class UserTransformer extends Transformer {
  transform(model) {
    return {
      id: model.id,
      email: model.email,
      status: model.status,
      created_at: model.created_at,
      updated_at: model.updated_at
    };
  }
}
```

<a name="incliding-data"></a>

### Including Data

Your transformer at this point is mainly just giving you a method to handle array conversion from your data source (or whatever your model is returning) to a simple array. Including data in an intelligent way can be tricky as data can have all sorts of relationships. Many developers try to find a perfect balance between not making too many HTTP requests and not downloading more data than they need to, so flexibility is also important.

Setup project

```javascript
// app/Transformers/UserTransformer.js
import Transformer from "./Transformer";

export default class UserTransformer extends Transformer {
  transform(model) {
    return {
      id: model.id,
      email: model.email,
      status: model.status,
      created_at: model.created_at,
      updated_at: model.updated_at
    };
  }

  includePosts(model) {
    return this.collection(model.posts, new PostTransformer());
  }
}
```

```javascript
// app/Transformers/PostTransformer.js
import Transformer from "./Transformer";

export default class PostTransformer extends Transformer {
  transform(model) {
    return {
      id: model.id,
      title: model.title,
      content: model.content,
      created_at: model.created_at,
      updated_at: model.updated_at
    };
  }
}
```

```javascript
//app/Repositories/UserRepository.js
import { Repository } from "./Repository";
import User from '../Models/User';

export default class UserRepository extends Repository {
  Models() {
    return User;
  }
}
```

```javascript
//app/Models/User.js
import Sequelize from "sequelize";
import sequelize from "../../config/sequelize";
import Post from "./Post";

const User = sequelize.define(
  "user",
  {
    email: {
      type: Sequelize.STRING,
      defaultValue: ""
    },
    password: {
      type: Sequelize.STRING,
      defaultValue: ""
    }
  },
  {
    tableName: "users",
    paranoid: false
  }
);

User.associate = models => {
  User.hasMany(Post);
};

export default User;
```

```javascript
//app/Models/Post.js
import Sequelize from "sequelize";
import sequelize from "../../config/sequelize";

const Post = sequelize.define(
  "post",
  {
    title: {
      type: Sequelize.STRING,
      defaultValue: ""
    },
    content: {
      type: Sequelize.STRING,
      defaultValue: ""
    }
  },
  {
    tableName: "posts",
    paranoid: false
  }
);

export default User;
```

And now to return user with their posts

```javascript
const users = App.make(UserRepository)
  .with(Post)
  .get();

res.json(ApiResponse.collection(users, new UserTransformer(["post"])));
```
