# Validation

- [Validation](#validation)
    - [Introduction](#introduction)
    - [Validation Quickstart](#validation-quickstart)
        - [Writing The Validation Logic](#writing-the-validation-logic)
        - [Use Validator](#use-validator)
        - [Custom Error Messages](#custom-error-messages)

<a name="introduction"></a>

## Introduction

We have several different approaches to validate your application's incoming data. By default, nsilly uses a `Validator` class which provides a convenient method to validate incoming HTTP request with a variety of powerful validation rules.

Validator is built on top of https://github.com/chriso/validator.js

> {tip} All rule can be found in `app/Validators/Validator.js`

<a name="validation-quickstart"></a>

## Validation Quickstart

### Writing The Validation Logic

All validator are located in `app/Validators` directory.

We can create new one by run following command:

```
yarn command make:validator {name}
```

Example

```
yarn command make:validator UserValidator
```

and UserValidator will be present in `app/Validators` directory

```javascript
import { AbstractValidator, REQUIRED, IS_EMAIL, IS_INT } from "./Validator";
import constants from "../../constants";
import { Exception } from "@nsilly/exceptions";
import _ from "lodash";

export const CREATE_USER_RULE = "create_user";
export const RULE_SAVE_USER_IMAGE = "save_user_image";
export const CHANGE_PASSWORD = "change_password";

export class UserValidator extends AbstractValidator {
  static getRules() {
    return {
      [CREATE_USER_RULE]: {
        email: [REQUIRED, IS_EMAIL],
        password: [REQUIRED, "min:6", "max:20"],
        first_name: [REQUIRED, "max:40"],
        last_name: [REQUIRED, "max:40"],
        gender: [
          gender => {
            if (!_.includes(constants.GENDER, gender)) {
              throw new Exception(
                `gender allow those value ${constants.GENDER.join(", ")} only`,
                1000
              );
            }
            return true;
          }
        ]
      },
      [RULE_SAVE_USER_IMAGE]: {
        url: [REQUIRED],
        type: [REQUIRED]
      },
      [CHANGE_PASSWORD]: {
        password: [REQUIRED, "min:6", "max:40"]
      }
    };
  }
}
```

<a name="use-validator"></a>

### Use Validator

The example bellow describe how to use Validator class

```javascript
import express from "express";
import bcrypt from "bcrypt-nodejs";
import UserTransformer from "../../../app/Transformers/UserTransformer";
import UserRepository from "../../../app/Repositories/UserRepository";
import { App } from "@nsilly/container";
import { Request, AsyncMiddleware } from "@nsilly/support";
import { ApiResponse } from "@nsilly/response";
import {
  UserValidator,
  CREATE_USER_RULE
} from "../../../app/Validators/UserValidator";

var router = express.Router();

async function store(req, res) {
  UserValidator.isValid(Request.all(), RULE_CREATE);

  const data = {
    email: Request.get("email"),
    password: bcrypt.hashSync(
      Request.get("password"),
      bcrypt.genSaltSync(8),
      null
    ),
    first_name: Request.get("first_name"),
    last_name: Request.get("last_name"),
    gender: Request.get("gender")
  };
  const user = await App.make(UserRepository).create(data);
  res.json(ApiResponse.item(user, new UserTransformer()));
}

router.post("/", AsyncMiddleware(store));

export default router;
```

<a name="custom-error-messages"></a>

### Custom Error Messages

If needed, you may use custom error messages for validation instead of the defaults. All error message can be updated in `app/Validators/Validator.js`
