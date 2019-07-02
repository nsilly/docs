# Repository

- [Repository](#repository)
  - [Introduction](#introduction)
  - [Working with Repository](#working-with-repository)
    - [Create a Repository](#create-a-repository)
    - [Methods](#methods)

<a name="introduction"></a>

## Introduction

Repository is used to abstract the data layer, making our application more flexible to maintain

> {tip} A Repository mediates between the domain and data mapping layers, acting like an in-memory domain object collection. Client objects construct query specifications declaratively and submit them to Repository for satisfaction. Objects can be added to and removed from the Repository, as they can from a simple collection of objects, and the mapping code encapsulated by the Repository will carry out the appropriate operations behind the scenes.

- Centralization of the data access logic makes code easier to maintain

- Business and data access logic can be tested separately

- Reduces duplication of code

- A lower chance for making programming errors

<a name="working-with-repository"></a>

## Working with Repository

<a name="create-a-repository"></a>

### Create a Repository

All repository class are located in `app/Repositories` directory. You can create new one by following command

```
yarn command make:repository {name}
```

Example

```
yarn command make:repository UserRepository --model=User
```

The class `UserRepository` will be present in `app/Repositories/UserRepository.js`

```javascript
import User from "../Models/User";
import { Repository } from "./Repository";

export default class UserRepository extends Repository {
  Models() {
    return User;
  }
}
```

<a name="methods"></a>

### Methods

- get()

- first()

- firstOrFail()

- paginate(per_page)

- findById(id)

- count()

- where(field, operator, value)

- orWhere(field, operator, value)

- whereIn(field, array values)

- whereNotIn(field, array values)

- create(attributes)

- update(attributes, id)

- firstOrCreate(attributes)

- updateOrCreate(attributes, values)

- delete()

- deleteById(id)

- skip(offset)

- take(limit)

- orderBy(field, direction)

- groupBy(field)

- with(relation)

- whereHas(relation, callback)

- withScope(scope)

- withTrashed()

> {tip} where(field, value) is a shortcut of where(field, '=', value), same as orWhere

#### get()

Execute the query as a "select" statement. Return the collection of resource.

@return Array

```javascript
const users = await App.make(UserRepository).get();
```

#### first()

Get the first record

@return Object

```javascript
const user = await App.make(UserRepository).first();
```

#### firstOrFail()

Execute the query and get the first result or throw an exception if no resource found

@return Object

```javascript
const user = await App.make(UserRepository).firstOrFail();
```

#### paginate(per_page = 15, page = 1)

Paginate the given query

@return LengthAwarePaginator

```javascript
const users = await App.make(UserRepository)
  .where("active", 1)
  .paginate();
```

#### findById(id)

Find a resource by its primary key.

@return Object

```javascript
const user = await App.make(UserRepository).findById(10);
```

#### count()

Retrieve the "count" result of the query.

@return Int

```javascript
const number_of_activated_user = await App.make(UserRepository)
  .where("active", 1)
  .count();
```

#### where(field, operator, value)

Add a basic "WHERE" clause to the query.

| Operator | Description       |
| -------- | ----------------- |
| =        | equal             |
| <=       | smaller and equal |
| >=       | larger and equal  |
| >        | larger            |
| <        | smaller           |
| like     | like              |
| <>       | different         |

```javascript
const users = await App.make(UserRepository)
  .where("name", "like", "joe")
  .get();
```

#### orWhere(field, operator, value)

Add a basic OR WHERE clause to the query.

```javascript
const users = await App.make(UserRepository)
  .where("status", 1)
  .orWhere("status", 3)
  .get();
```

#### whereIn(field, array values)

Add an "WHERE IN" clause to the query.

```javascript
const users = await App.make(UserRepository)
  .whereIn("id", [1, 3])
  .get();
```

#### whereNotIn(field, array values)

Add an "WHERE NOT IN" clause to the query.

```javascript
const users = await App.make(UserRepository)
  .whereNotIn("id", [1, 3])
  .get();
```

#### create(attributes)

Save a new model and return the instance.

@return Object

```javascript
const user = await App.make(UserRepository).create({
  email: "user@example.com",
  name: "Joe"
});
```

#### update(attributes, id = undefined)

@return Object | Boolean

Update multiple instances that match the where options or update resource with given id

```javascript
const user = await App.make(UserRepository).update(
  {
    email: "user@example.com",
    name: "Joe"
  },
  1
);

await App.make(UserRepository)
  .where("active", 1)
  .update({
    note: "activated"
  });
```

#### firstOrCreate(attributes)

@return Object

Save a new model or create new instance if not exist.

Return first user with name "Joe" if it's existing in database. If not exist create new user with email and name given.

```javascript
const user = await App.make(UserRepository).firstOrCreate(
  {
    name: "Joe"
  },
  { email: "user@example.com", name: "Joe" }
);
```

#### updateOrCreate(attributes, values)

@return Object

Create or update a record matching the attributes, and fill it with values

If a user with email is `user@example.com` existing in database then update it with email and name give

If a user with email is `user@example.com` is not exist in database then create new one with email and name give

```javascript
const user = await App.make(UserRepository).firstOrCreate(
  {
    email: "user@example.com"
  },
  { email: "user@example.com", name: "Joe" }
);
```

#### delete()

Delete resources by given condition

@return Boolean

```javascript
await App.make(UserRepository)
  .where("status", 0)
  .delete();
```

#### deleteById(id)

Delete resource by given id

@return Boolean

```javascript
await App.make(UserRepository)
  .where("status", 0)
  .deleteById(1);
```

#### skip(offset)

Set the "offset" value of the query.

```javascript
const users = await App.make(UserRepository)
  .skip(5)
  .get();
```

#### take(limit)

Set the "limit" value of the query.

```javascript
const users = await App.make(UserRepository)
  .skip(5)
  .take(10)
  .get();
```

#### orderBy(field, direction)

Add an "order by" clause to the query.

```javascript
const users = await App.make(UserRepository)
  .orderBy("id", "DESC")
  .orderBy("status", "ASC")
  .get();
```

#### groupBy(field)

Add an "GROUP BY" clause to the query.

```javascript
const users = await App.make(UserRepository)
  .groupBy("status")
  .get();
```

#### with(relation)

Begin querying a model with eager loading.

```javascript
const users = await App.make(UserRepository)
  .with(Post)
  .get();
```

#### whereHas(relation, callback)

Add a basic where clause with relation to the query.

```javascript
const users = await App.make(UserRepository)
  .whereHas(Post, function(q) {
    q.where("title", "like", "news").where("status", 1);
    return q;
  })
  .get();
```

#### withScope(scope)

Add scope to the query

```javascript
const users = await App.make(UserRepository)
  .withScope("activated_posts")
  .get();
```

#### withTrashed()

Include deleted records

```javascript
const users = await App.make(UserRepository)
  .withTrashed()
  .get();
```
