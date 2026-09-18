# project setup

Create a minimal console or command-line project in your chosen language. Do not add an HTTP framework. A networking or socket module from the language's standard library is ideal.

Use a small structure such as:

```
http-server/
├── source file(s)
└── tests/
```

How you run it depends on the language. The result should be a process that stays running and prints:

```
listening on http://127.0.0.1:4221
```

Then make a request from another terminal:

```bash
curl -i http://127.0.0.1:4221/
```

## First milestone

Start with a listener that accepts connections. Then add the pipeline one stage at a time:

```
listener → request reader → parser → router → response writer
```

Keeping these as separate modules or functions prevents socket I/O, protocol parsing, and application behavior from becoming one large function. It also makes the parser and router testable without opening a network port.

Use the [language mapping worksheet](/broken/pages/cff9d4f65bf0b71aee1a7064589b6b473c63dc3d) before translating the [complete pseudocode](/broken/pages/22d6f58bd3b9fce4770399bd538af63624872097).
