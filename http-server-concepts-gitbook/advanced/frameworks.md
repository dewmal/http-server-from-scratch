# frameworks

The server built in this book reveals the same conceptual pipeline used by frameworks:

```
socket → HTTP parser → request type → router → handler → response type → socket
```

Libraries and frameworks add tested implementations and reusable layers:

* an HTTP engine handles connections, parsing, and message semantics;
* a concurrency runtime or server host schedules work;
* a router maps methods and paths to handlers;
* middleware adds concerns such as tracing, authentication, and timeouts.

What looks like this in the learning server:

```
IF request.target == "/" THEN HOME()
ELSE IF request.target == "/user-agent" THEN USER_AGENT(request)
ELSE NOT_FOUND()
```

becomes something like this in a framework:

```
router.GET("/", HOME)
router.GET("/user-agent", USER_AGENT)
```

The abstraction does not erase the fundamentals. It packages them. When a framework reports an invalid body, connection timeout, missing header, or handler backpressure, the concepts in this book explain what is happening underneath.

## What to learn next

{% stepper %}
{% step %}
## Build the standard-library example

Trace a request end to end.
{% endstep %}

{% step %}
## Rebuild the routes

Use a popular framework in your chosen language.
{% endstep %}

{% step %}
## Add JSON extraction

Add typed error responses.
{% endstep %}

{% step %}
## Add middleware

Add middleware for tracing and timeouts.
{% endstep %}

{% step %}
## Compare responsibilities

Compare the manual responsibilities with the framework-provided ones.
{% endstep %}
{% endstepper %}
