# Requests

- [Requests](#requests)
    - [Accessing The Request](#accessing-the-request)

<a name="accessing-the-request"></a>

## Accessing The Request

nsilly provide a convenient way to get data of a request, all query params and body params will be merge and available in `Request` object.

> {tip} Only query and body of express request object are merged

```javascript
import { Request } from "@nsilly/support";

const data = {
  email: Request.get("email"),
  name: Request.get("name")
};
```

Get all params:

```javascript
import { Request } from "@nsilly/support";

const data = Request.all();
```

Get single param and return default value if it's not present:

```javascript
import { Request } from "@nsilly/support";

const name = Request.get("name", "Joe");
```

Check a param is present or not:

```javascript
import { Request } from "@nsilly/support";

const hasName = Request.has("name");
```
