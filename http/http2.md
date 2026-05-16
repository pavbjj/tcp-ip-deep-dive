# HTTP/2 Keepalive and GOAWAY Explained

This document explains how HTTP/2 keeps connections alive and how graceful shutdown works using PING and GOAWAY frames.

---

# Does HTTP/2 have keepalive?

Yes — but not a single built-in keepalive mechanism like a timer. Instead, HTTP/2 relies on multiple mechanisms depending on implementation (browser, proxy, gRPC, etc.).

---

# PING frames (primary keepalive)

HTTP/2 defines a PING frame used to:

- Check if the peer is alive
- Measure round-trip time (RTT)
- Prevent idle connection timeouts in NATs, firewalls, and load balancers

A typical exchange looks like:

PING  
PING ACK  

PING frames are always sent on stream 0 (connection-level) and carry 8 opaque bytes that must be echoed back unchanged.

They are heavily used in systems like gRPC, service meshes, and long-lived API connections.

---

# TCP keepalive (below HTTP/2)

Even if HTTP/2 is idle, the operating system may send TCP keepalive probes.

This is controlled by OS-level settings such as:

- tcp_keepalive_time  
- tcp_keepalive_intvl  
- tcp_keepalive_probes  

This operates below HTTP/2 and is independent of the protocol itself.

---

# Application-level traffic

Some systems maintain connection liveness using periodic HTTP/2 control frames such as:

- PING
- WINDOW_UPDATE
- SETTINGS

These are implementation-specific and not strictly required by the protocol.

---

# What is GOAWAY?

GOAWAY is an HTTP/2 frame used for graceful shutdown of a connection.

It does NOT immediately close the TCP connection.

Instead, it tells the peer:

> Stop creating new streams. I am shutting down this connection.

---

# Scope of GOAWAY

GOAWAY applies to the entire HTTP/2 connection:

- It is always sent on stream 0
- It is not tied to any individual stream
- It affects all future stream creation on the connection

---

# GOAWAY structure

A GOAWAY frame contains:

- Last Stream ID
- Error Code
- Optional debug data

Example:

GOAWAY last_stream_id=5

---

# Meaning of Last Stream ID

If a server sends:

GOAWAY last_stream_id=5

It means:

- Streams with ID ≤ 5 may have been processed
- Streams with ID > 5 must NOT be created
- Clients should retry new requests on a new connection

Importantly, it does NOT cancel existing streams.

---

# Example lifecycle

Before GOAWAY:

stream 1  
stream 3  
stream 5  
stream 7  

GOAWAY is sent:

GOAWAY last_stream_id=5  

After GOAWAY:

- Streams 1, 3, 5 continue normally
- Stream 7 is invalid and must be retried
- No new streams are allowed on this connection

---

# GOAWAY vs RST_STREAM

These two are often confused:

| Frame | Scope | Purpose |
|------|------|---------|
| GOAWAY | Entire connection | Stop new streams, graceful shutdown |
| RST_STREAM | Single stream | Cancel one request |

---

# RST_STREAM example

RST_STREAM stream=5

Used for:

- Request cancellation
- Timeouts
- Errors
- Client aborts

RST_STREAM affects only one stream while the connection remains active.

---

# Why GOAWAY exists

HTTP/2 multiplexes many requests over a single TCP connection. Without GOAWAY, graceful shutdown would be impossible.

GOAWAY allows:

- Safe draining of traffic
- Rolling deployments
- Load balancer rotation
- Connection lifecycle control

---

# Common reasons for GOAWAY

## Idle timeout
Load balancers or proxies close idle connections after a timeout.

## Deployment or restart
Servers like NGINX, HAProxy, Envoy, and Kubernetes ingress controllers use GOAWAY during shutdown:

1. Send GOAWAY
2. Stop accepting new streams
3. Allow active streams to complete
4. Close TCP connection

## Connection aging
Connections may be rotated to:

- Rebalance load
- Refresh TLS/session state
- Avoid very long-lived connections
- Reduce congestion risk

## Errors or protocol issues
GOAWAY may include error codes such as:

- NO_ERROR
- PROTOCOL_ERROR
- FLOW_CONTROL_ERROR
- ENHANCE_YOUR_CALM

---

# GOAWAY vs TCP FIN

HTTP/2 GOAWAY is an application-layer signal, while TCP FIN is transport-layer closure.

Typical sequence:

GOAWAY  
(active streams finish)  
TCP FIN  



