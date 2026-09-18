# responses

An HTTP/1.1 response mirrors the request structure:

```http
HTTP/1.1 200 OK
Content-Type: text/plain; charset=utf-8
Content-Length: 5
Connection: close

Hello
```

It consists of:

```
status line → headers → blank line → optional body
```

The status line contains the HTTP version, numeric status code, and reason phrase.

| Range | Category      | Examples                                                     |
| ----- | ------------- | ------------------------------------------------------------ |
| `1xx` | informational | `100 Continue`                                               |
| `2xx` | success       | `200 OK`, `201 Created`, `204 No Content`                    |
| `3xx` | redirection   | `301 Moved Permanently`, `302 Found`                         |
| `4xx` | client error  | `400 Bad Request`, `404 Not Found`, `405 Method Not Allowed` |
| `5xx` | server error  | `500 Internal Server Error`                                  |

A `404` does not mean HTTP failed. It means the server successfully understood enough of the request to report that no matching resource exists.

## Body rules

`Content-Length` counts bytes, not characters. Some responses must not contain a body, including responses to `HEAD` and statuses such as `204`. A complete implementation must account for method- and status-specific rules.
