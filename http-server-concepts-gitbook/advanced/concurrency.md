# concurrency

The companion server processes connections sequentially:

```
accept A → finish A → accept B → finish B
```

A slow client can therefore delay everyone behind it. There are several common models.

## Thread per connection

```
LOOP forever
    connection = ACCEPT(listener)
    START_THREAD_OR_TASK(HANDLE_CONNECTION, connection)
END
```

This is easy to understand, but an unbounded number of clients can create an unbounded number of OS threads. Threads consume stack memory and scheduler resources.

## Fixed worker pool

```
accepted connections
         │
         ▼
     bounded queue
    ┌────┼────┐
    ▼    ▼    ▼
 worker worker worker
```

A pool limits concurrency and resource use. A bounded queue also creates backpressure instead of allowing pending work to grow forever.

## Async I/O

An async runtime or event loop lets many tasks share a smaller number of OS threads:

```
socket A waiting ─┐
socket B ready ───┼─► runtime schedules useful work
socket C waiting ─┤
socket D ready ───┘
```

Async does not make CPU work free. Blocking file or CPU-heavy work can still stall a runtime worker and should be isolated appropriately.

{% hint style="info" %}
Concurrency also requires timeouts, connection limits, graceful shutdown, panic isolation, and careful shared-state design.
{% endhint %}
