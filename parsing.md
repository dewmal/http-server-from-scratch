# parsing

Parsing has two phases. First, read enough bytes to find `\r\n\r\n`. Second, interpret the complete header block.

## Read until the boundary

```
FUNCTION FIND_HEADER_END(bytes)
    RETURN INDEX_OF_BYTE_SEQUENCE(bytes, [13, 10, 13, 10])
END
```

Call `read()` repeatedly, append only the returned bytes, and stop when the boundary appears. Enforce a maximum header size. A single fixed-size read is incorrect because TCP can split the header anywhere.

## Parse the request head

Once the entire head is buffered:

{% stepper %}
{% step %}
## Decode the header section

Decode that header section as text.
{% endstep %}

{% step %}
## Split the lines

Split lines on `\r\n`.
{% endstep %}

{% step %}
## Parse the request line

Parse exactly three request-line fields.
{% endstep %}

{% step %}
## Validate the version

Validate the supported version.
{% endstep %}

{% step %}
## Parse and normalize headers

Split each header at its first colon.

Normalize header names to lowercase.
{% endstep %}
{% endstepper %}

```
Request
    method: text
    target: text
    version: text
    headers: map of lowercase text names to text values
    body: bytes
```

The complete example only accepts HTTP/1.1 request heads and does not read bodies. A fuller parser would also validate `Host`, resolve body framing, preserve duplicate headers where meaningful, distinguish the path from query data, and safely decode percent-encoding.

## Failure is part of the protocol

Malformed input should become a `400 Bad Request`, not a crash or uncaught exception. Treat all bytes from the network as untrusted and make every assumption explicit.
