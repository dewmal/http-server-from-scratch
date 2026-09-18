# security production

The learning server is intentionally incomplete. Do not expose it directly to an untrusted network.

A production HTTP server needs defenses at every boundary:

| Area        | Typical controls                                     |
| ----------- | ---------------------------------------------------- |
| Input size  | header, body, URL, and field-count limits            |
| Time        | read, write, idle, and handler deadlines             |
| Connections | global and per-client limits; bounded queues         |
| Parsing     | ambiguity rejection; duplicate/framing validation    |
| Files       | traversal prevention; least privilege; upload limits |
| Transport   | TLS configuration and certificate management         |
| Operations  | structured logs, metrics, tracing, health checks     |
| Lifecycle   | graceful shutdown and draining                       |

Particularly dangerous parsing ambiguities involve conflicting `Content-Length` values or disagreement between `Content-Length` and `Transfer-Encoding`. Intermediaries that interpret a message differently can enable request smuggling. Mature libraries have accumulated years of protocol and security fixes for these cases.

## Safe next step

Keep the application concepts—structured requests, routing, handlers, structured responses—but delegate wire parsing and connection management to a maintained library. Using a library simply moves the protocol boundary to well-tested code.
