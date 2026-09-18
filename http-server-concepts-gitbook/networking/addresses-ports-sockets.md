# addresses ports sockets

A server waits at an **IP address and port**:

```
127.0.0.1:4221
└───┬───┘ └┬─┘
 address   port
```

The address identifies a network interface. `127.0.0.1` is the IPv4 loopback address, so only programs on the same machine can reach it. `0.0.0.0` means all local IPv4 interfaces and may expose the server to other machines, depending on firewall and network rules.

The port identifies a service on that address:

```
machine
├── 22    SSH
├── 80    HTTP
├── 443   HTTPS
├── 5432  PostgreSQL
└── 4221  this learning server
```

An endpoint is often represented as a **socket address**. A connected TCP conversation is uniquely described by both endpoints:

```
(client IP, client port) ↔ (server IP, server port)
```

Many clients can therefore connect to port `4221` at the same time because their client endpoints differ.

## Binding

Calling your language's TCP `bind` operation for `127.0.0.1:4221` asks the operating system to reserve that local endpoint for the process. Binding can fail when:

* Another process already owns the port.
* The address is not available.
* Permissions or security policy reject it.

The operating system, not the programming language, delivers incoming connections to the process that owns the listening socket.
