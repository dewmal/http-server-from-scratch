# routing

Routing maps a parsed request to application behavior. The lookup normally uses both the method and request target:

```
Request
   │
   ├── GET /             → home handler
   ├── GET /echo/{value} → echo handler
   ├── GET /user-agent   → header handler
   ├── other method      → 405
   └── other target      → 404
```

A small router can use pattern matching:

```
FUNCTION ROUTE(request)
    IF request.method != "GET"
        RETURN TEXT_RESPONSE(405, "Method Not Allowed")

    IF request.target == "/"
        RETURN TEXT_RESPONSE(200, "Hello from your server!")

    IF request.target == "/user-agent"
        value = GET_OR_DEFAULT(request.headers, "user-agent", "unknown")
        RETURN TEXT_RESPONSE(200, value)

    IF STARTS_WITH(request.target, "/echo/")
        value = REMOVE_PREFIX(request.target, "/echo/")
        RETURN TEXT_RESPONSE(200, value)

    RETURN TEXT_RESPONSE(404, "Not Found")
END
```

Use a safe prefix-removal operation rather than slicing with a magic index. Production routing also handles query strings, percent-encoded path segments, method-specific fallbacks, parameters, and middleware.

## `404` versus `405`

* `404 Not Found`: no resource matches the target.
* `405 Method Not Allowed`: the resource exists conceptually, but not for this method.

The example uses a simplified global `405` for non-GET methods. A production router should decide allowed methods per route and include an `Allow` header.
