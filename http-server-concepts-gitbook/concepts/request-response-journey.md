# request response journey

Entering `http://127.0.0.1:4221/hello` does not call a function in your program directly. The browser and server cooperate through layers:

```
Browser                          Server process
   │                                  │
   │  connect to 127.0.0.1:4221      │
   ├─────────────────────────────────►│ listening socket accepts
   │                                  │
   │  GET /hello HTTP/1.1\r\n...     │
   ├─────────────────────────────────►│ read + parse
   │                                  │ route + handle
   │  HTTP/1.1 200 OK\r\n...         │
   │◄─────────────────────────────────┤ write
   │                                  │
```

The request might be:

```http
GET /hello HTTP/1.1
Host: 127.0.0.1:4221
User-Agent: curl/8.7.1
Accept: */*
```

The response might be:

```http
HTTP/1.1 200 OK
Content-Type: text/plain; charset=utf-8
Content-Length: 5
Connection: close

Hello
```

The operating system handles packets, retransmission, and delivery to the socket. Your program receives a connected socket, reads bytes, applies HTTP rules, and writes bytes back.

## The server loop

A simple server has two responsibilities:

```
accept connections ──► process each connection
```

Accepting creates a connected stream. Processing the stream reads a request, chooses an action, and sends a response. Keeping these responsibilities separate makes later concurrency changes much easier.
