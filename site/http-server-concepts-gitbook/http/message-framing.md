# message framing

HTTP/1.x lines conventionally end with CRLF:

```
\r = carriage return
\n = line feed
```

The empty line separating headers from the body is therefore four bytes:

```
\r\n\r\n
```

A parser first searches for this header terminator:

```
[request line + headers]\r\n\r\n[body bytes]
                       ▲
                       header boundary
```

Finding it tells you where the body starts, not necessarily where it ends. Body framing can use:

* `Content-Length`, which specifies an exact byte count;
* chunked transfer coding, which frames a sequence of chunks;
* connection closure in cases where the protocol permits it.

The companion server only accepts bodyless `GET` requests and closes the connection after the response. A body-capable reader needs a loop:

```
read until header terminator
        │
        ▼
parse and validate Content-Length
        │
        ▼
read until body_length bytes are buffered
        │
        ▼
leave any extra bytes for the next message
```

Set maximum header and body sizes before allocating or looping. Otherwise a slow or oversized request can consume unbounded time or memory.
