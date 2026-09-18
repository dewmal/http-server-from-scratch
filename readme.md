# README

This book teaches how to build a small HTTP/1.1 server in **any programming language**. The protocol, networking model, and architecture are language-independent; you choose the language and translate the pseudocode into its socket and byte APIs.

At the lowest level, a server performs this pipeline:

```
client
  │ TCP connection
  ▼
read bytes → frame a message → parse HTTP → route → build response
                                                        │
client ◀────────────── write response bytes ◀───────────┘
```

By the end, you will understand:

* how an IP address and port identify a listening program;
* why TCP supplies bytes, not requests;
* how HTTP/1.1 gives those bytes message structure;
* why one `read()` is not guaranteed to return one request;
* how routing, file serving, concurrency, compression, and keep-alive fit together;
* where a learning server ends and production engineering begins.

## Choose your language

Use a language with TCP socket and byte-array support. C, C++, C#, Go, Java, JavaScript/Node.js, Kotlin, Python, Ruby, Rust, Swift, and many others all work. You do not need an HTTP framework for the learning implementation because the point is to see the layer a framework normally provides.

The book uses structured pseudocode with familiar operations such as `BIND`, `ACCEPT`, `READ`, and `WRITE_ALL`. The [language mapping guide](/broken/pages/1259a09bbdacc2b0235ad8821f1ae51362db2112) helps you locate their equivalents.

## What you will build

The companion server supports:

| Request            | Result                          |
| ------------------ | ------------------------------- |
| `GET /`            | A welcome message               |
| `GET /echo/{text}` | Echoes the path value           |
| `GET /user-agent`  | Returns the `User-Agent` header |
| anything else      | `404 Not Found`                 |

## Quick start

Choose a language, create a console application, and translate the [complete server pseudocode](/broken/pages/4408b7ae9e7f6a17071b75ae93e52d3a8d2ce9e2).

{% stepper %}
{% step %}
## Run the server

Run your translated server implementation.
{% endstep %}

{% step %}
## Send requests from another terminal

```bash
curl -i http://127.0.0.1:4221/
curl -i http://127.0.0.1:4221/echo/hello
curl -i http://127.0.0.1:4221/user-agent
```
{% endstep %}
{% endstepper %}

Start with [How to use this book](/broken/pages/56c9c38ca3c2b39234c1007c4ca754b5087d6997), or jump to the [complete implementation](/broken/pages/4408b7ae9e7f6a17071b75ae93e52d3a8d2ce9e2).

{% hint style="warning" %}
Your learning implementation will deliberately support only a limited subset of HTTP/1.1. Bind to the loopback address and do not expose it to an untrusted network.
{% endhint %}
