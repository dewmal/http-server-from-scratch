# tcp byte stream

TCP preserves byte order, but not application message boundaries. A sender may make one write while the receiver needs several reads:

```
sender writes 5000 bytes

receiver read #1: 2048 bytes
receiver read #2: 2048 bytes
receiver read #3:  904 bytes
```

The reverse is also possible: one read can contain bytes from more than one HTTP message on a persistent connection.

{% hint style="info" %}
One `read()` is not one request.
{% endhint %}

`read()` returns the number of bytes placed in the buffer. Only that prefix is valid new data:

```
buffer = NEW_BYTE_ARRAY(1024)
bytes_read = READ(connection, buffer)
received = SLICE(buffer, 0, bytes_read)
```

A return value of `0` means the peer closed its sending side and no more bytes will arrive.

## The framing problem

TCP produces this:

```
byte byte byte byte byte byte byte ...
```

HTTP wants this:

```
[request 1][request 2][request 3]
```

The HTTP parser must find the end of the header section and then determine the body length. It may need to retain unused bytes for the next request. This translation from stream to messages is called **framing**.
