# tcp listener stream

Every general-purpose networking library exposes the same two ideas, although names differ:

* A **listening socket** waits for new connections.
* A **connected socket** represents one bidirectional byte stream.

```
listener = TCP_LISTENER()
BIND(listener, "127.0.0.1", 4221)
LISTEN(listener)

LOOP forever
    TRY
        connection = ACCEPT(listener)
        PRINT "connected: " + PEER_ADDRESS(connection)
    CATCH error
        LOG "accept failed: " + error
    END
END
```

The loop does not receive HTTP requests. It receives TCP connections. Once accepted, the connected socket supports byte reads and writes:

```
client                     server
   ─────── request bytes ───────►
   ◄────── response bytes ───────
```

Give one handler clear responsibility for closing each accepted connection. Languages may express this using scoped cleanup, context managers, `defer`, `finally`, or explicit `close` calls.

## Graceful error handling

{% hint style="warning" %}
Avoid `unwrap()` in a long-running accept loop. One failed connection should not terminate the entire server.
{% endhint %}

Handle per-connection errors, log them, and continue accepting.
