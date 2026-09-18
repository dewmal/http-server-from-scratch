# testing

Test the parser and router as ordinary functions. These tests are fast and do not need a port:

```
TEST "finds a complete header"
    input = ASCII_BYTES("GET / HTTP/1.1\r\n\r\nbody")
    ASSERT_EQUAL(FIND_HEADER_END(input), 14)
END
```

Use your language's normal test runner. Keep parser and router tests independent of the network so they stay fast and deterministic.

For end-to-end inspection, use:

```bash
curl -v http://127.0.0.1:4221/echo/debug
```

The lines prefixed with `>` are request data; lines prefixed with `<` are response data.

You can also send an exact request using a TCP client such as `nc`:

```bash
printf 'GET / HTTP/1.1\r\nHost: localhost\r\n\r\n' | nc 127.0.0.1 4221
```

Useful failure cases include:

* An incomplete header
* Unsupported version
* Malformed header
* Oversized header
* Unknown route
* Unsupported method

A server is defined as much by how it rejects invalid input as by its successful responses.
