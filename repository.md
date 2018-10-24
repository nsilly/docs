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
