# files

HTTP can expose ordinary filesystem operations:

```
GET /files/readme.txt
        │
        ▼
read files/readme.txt
        │
        ▼
200 + file bytes
```

Use a binary file-reading API because a file need not be UTF-8:

```
body = READ_FILE_AS_BYTES("files/readme.txt")
```

A `POST /files/name` endpoint could write request body bytes with a binary file-writing API and return `201 Created`.

## Never join an untrusted path blindly

This is unsafe:

```
path = "files/" + requested_name
```

An attacker may request `../../secret`. This is **path traversal**. A safe design should:

* map URLs to known identifiers where possible;
* reject absolute paths and parent components such as `..`;
* canonicalize both the allowed root and candidate, then verify containment;
* decide how symbolic links are handled;
* use least-privilege filesystem permissions;
* cap upload sizes and avoid overwriting files accidentally.

Also choose a correct `Content-Type`. `application/octet-stream` is a safe generic binary type, but production servers commonly infer a media type from trusted metadata or a validated extension.
