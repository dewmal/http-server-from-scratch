# persistent connections

HTTP/1.1 connections are persistent by default unless either side requests closure. One TCP connection may carry multiple request-response exchanges:

```
connect
  ├─ request 1 → response 1
  ├─ request 2 → response 2
  └─ request 3 → response 3
close
```

This avoids repeated TCP connection setup. It also complicates the server:

* After one message, unread bytes may already contain the next one.
* Every message needs unambiguous body framing.
* The server needs idle timeouts and request-count limits.
* Connection-close rules must be honored.
* HTTP/1.1 pipelining can place multiple requests in flight.

A conceptual loop is:

```
LOOP
    request = READ_ONE_REQUEST(buffered_connection)
    should_close = REQUEST_WANTS_CLOSE(request)
    WRITE_RESPONSE(buffered_connection, ROUTE(request), should_close)
    IF should_close
        BREAK
END
```

The important word is **buffered**. If a read collects the end of request one and the beginning of request two, the extra bytes must be retained rather than discarded.

The companion implementation sends `Connection: close`, making one request per connection an explicit design choice.
