# building responses

A response should own its status, headers, and raw body bytes:

```
Response
    status_code: integer
    reason_phrase: text
    content_type: text
    body: bytes
```

The writer serializes metadata first and body bytes second:

```
head_text = "HTTP/1.1 " + status_code + " " + reason_phrase + CRLF
head_text += "Content-Type: " + content_type + CRLF
head_text += "Content-Length: " + BYTE_LENGTH(body) + CRLF
head_text += "Connection: close" + CRLF
head_text += CRLF

WRITE_ALL(connection, ENCODE_UTF8(head_text))
WRITE_ALL(connection, body)
FLUSH_IF_REQUIRED(connection)
```

Use a “write all” API or implement a loop, because a single socket write may accept fewer bytes than provided.

`Connection: close` documents the example's lifecycle: one request, one response, then close. This also gives an unambiguous end to the exchange.

## Why body bytes are separate

Formatting the whole response as text only works for text bodies. Keeping the body as raw bytes allows images, compressed data, and other binary content. It also makes `Content-Length` a direct byte count.
