# bytes text protocols

Networks move bytes. A protocol gives those bytes shared meaning.

The ASCII bytes:

```
71 69 84 32 47
```

represent:

```
G  E  T     /
```

TCP does not know that `GET` is a method. HTTP defines that meaning. This is layering:

```
HTTP                 methods, headers, status codes, bodies
──────────────────────────────────────────────────────────
TCP                  reliable ordered byte stream
──────────────────────────────────────────────────────────
IP                   addressing and packet routing
──────────────────────────────────────────────────────────
Ethernet / Wi-Fi     local link transmission
```

## Text strings versus byte arrays

Most languages distinguish text strings from raw byte arrays, even if their type names differ. Network messages and files are arbitrary bytes, so keep data as bytes until a field is known to be text in a specific encoding.

This matters for:

* binary response bodies such as images;
* incomplete multi-byte UTF-8 characters split across reads;
* malformed or hostile input;
* byte-accurate `Content-Length` calculations.

For a text body, encode it first and count the resulting bytes rather than counting characters. The word `café` contains four characters but five UTF-8 bytes.
