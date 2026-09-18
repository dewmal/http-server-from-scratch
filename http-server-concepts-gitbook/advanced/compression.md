# compression

A client advertises response encodings it can decode:

```http
Accept-Encoding: gzip, br
```

If the server selects gzip, it compresses the representation and describes the result:

```http
Content-Encoding: gzip
Vary: Accept-Encoding
Content-Length: 37
```

```
original body → gzip encoder → compressed body bytes → client decoder
```

`Accept-Encoding` is a request preference. `Content-Encoding` describes the encoding actually applied to the response body. After compression, `Content-Length` must be the compressed byte length.

Compression often helps HTML, CSS, JavaScript, JSON, XML, and plain text. It may waste CPU for already-compressed formats such as JPEG, PNG, MP4, and ZIP, or for very small bodies.

## Negotiation is more than substring matching

Encoding values can include case variations, lists, wildcards, and quality weights such as `gzip;q=0`. A correct implementation parses those rules, does not choose an explicitly unacceptable encoding, and includes `Vary: Accept-Encoding` when caches may store different representations.

{% hint style="warning" %}
Compression also has security considerations when secrets and attacker-controlled text share a compressed response. Use established middleware instead of inventing production negotiation logic.
{% endhint %}
