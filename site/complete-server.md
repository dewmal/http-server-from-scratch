# complete server

The language-neutral blueprint combines the pieces without hiding the boundaries between them:

```
main
 └─ accept connected socket
     └─ handle_connection
         ├─ read_request
         │   ├─ find_header_end
         │   └─ parse_request_head
         ├─ route
         └─ Response::write_to
```

Translate [`examples/pseudocode/server.pseudocode`](file:///2350534/examples/pseudocode/server.pseudocode) into your chosen language. Preserve the byte-oriented behavior and function boundaries; syntax and naming can follow the conventions of your language.

Try each route:

```bash
curl -i http://127.0.0.1:4221/
curl -i http://127.0.0.1:4221/echo/hello
curl -i -A "book-client" http://127.0.0.1:4221/user-agent
curl -i http://127.0.0.1:4221/missing
curl -i -X POST http://127.0.0.1:4221/
```

## Deliberate limitations

The server is small enough to audit because it does **not** yet implement request bodies, chunked transfer coding, percent-decoding, query parsing, keep-alive, timeouts, TLS, or concurrent processing. These are learning boundaries, not production recommendations.

The next useful exercises are:

{% stepper %}
{% step %}
## Add a `HEAD /` response with headers but no body
{% endstep %}

{% step %}
## Add a typed `Method` enum
{% endstep %}

{% step %}
## Add request-body reading with a strict size limit
{% endstep %}

{% step %}
## Add a fixed worker pool
{% endstep %}

{% step %}
## Replace the manual protocol layer with a mature HTTP library or framework in your language and compare the architecture
{% endstep %}
{% endstepper %}
