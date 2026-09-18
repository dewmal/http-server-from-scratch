# choosing a language

The best language is one you want to learn and can run locally. The protocol does not change with the language.

Your language or standard library needs equivalents for these capabilities:

| Concept used in this book | Capability to find                        |
| ------------------------- | ----------------------------------------- |
| `BIND` and `LISTEN`       | Create a TCP server socket                |
| `ACCEPT`                  | Accept one incoming connection            |
| `READ`                    | Read bytes from a connected socket        |
| `WRITE_ALL`               | Keep writing until all bytes are sent     |
| byte array                | Store arbitrary binary data               |
| byte search               | Locate `\r\n\r\n` in buffered data        |
| text decode/encode        | Convert explicitly between bytes and text |
| map/dictionary            | Store normalized header names and values  |

## Suggested search terms

Search your language documentation for “TCP server socket,” “read bytes from socket,” and “write all bytes.” Avoid an HTTP server library for the first version: it would correctly do the exact parsing you are trying to learn.

## Implementation worksheet

Before coding, fill in this mapping:

| Operation                 | Your language/API |
| ------------------------- | ----------------- |
| Create listener           |                   |
| Bind to `127.0.0.1:4221`  |                   |
| Accept connection         |                   |
| Read bytes                |                   |
| Write all bytes           |                   |
| Close connection safely   |                   |
| Find byte sequence        |                   |
| Decode header bytes       |                   |
| Start a thread/task later |                   |

Use explicit byte APIs. Some convenient text-oriented socket readers normalize line endings, buffer ahead, or stop on delimiters; those behaviors can hide the framing lessons this project is meant to teach.
