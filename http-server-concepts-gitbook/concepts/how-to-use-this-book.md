# how to use this book

The chapters are arranged in layers. Read them in order the first time:

```
mental model → TCP → HTTP syntax → parser → router → server → advanced topics
```

Each layer answers a different question:

{% stepper %}
{% step %}
## Networking

How do bytes reach this process?
{% endstep %}

{% step %}
## HTTP

What do those bytes mean?
{% endstep %}

{% step %}
## Parsing

Where are the method, path, headers, and body?
{% endstep %}

{% step %}
## Routing

Which application behavior should run?
{% endstep %}

{% step %}
## Response writing

How does the result become bytes again?
{% endstep %}
{% endstepper %}

## Learning strategy

Run the example after each implementation chapter. Use `curl -i` because `-i` shows the response status and headers as well as the body. Add `-v` when you want to see both sides of the exchange.

Keep this distinction in mind throughout the book:

> TCP transports an ordered stream of bytes. HTTP defines how applications interpret those bytes as messages.

## Scope

The companion server intentionally handles one request per connection and sends `Connection: close`. This keeps the first implementation honest and understandable. Later chapters explain what is required for persistent connections, request bodies, concurrency, and defensive limits.
