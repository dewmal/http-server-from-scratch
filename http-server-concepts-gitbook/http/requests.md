# requests

An HTTP/1.1 request contains a request line, headers, a blank line, and an optional body:

```http
POST /users HTTP/1.1
Host: localhost:4221
Content-Type: application/json
Content-Length: 15

{"name":"Ada"}
```

```
POST /users HTTP/1.1       request line
Host: localhost:4221       header
Content-Type: ...          header
Content-Length: 15         header
                            blank line
{"name":"Ada"}            body
```

## Request line

The request line has three fields:

```
POST          /users          HTTP/1.1
method        request target  version
```

Common methods include `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, and `OPTIONS`. A route is normally selected by **method and target**, so `GET /users/10` and `DELETE /users/10` are different operations.

## Headers

Headers are metadata written as `Name: value`. Names are case-insensitive. Examples include:

* `Host`: requested authority; required in HTTP/1.1 requests.
* `Content-Length`: body length in bytes.
* `Content-Type`: media type of the body.
* `User-Agent`: information supplied by the client.
* `Accept-Encoding`: response encodings the client can decode.

{% hint style="info" %}
Real header parsing has subtleties. A learning parser can normalize names to lowercase, but production code should use a tested HTTP library.
{% endhint %}
