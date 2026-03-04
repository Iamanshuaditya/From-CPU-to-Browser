# Week 1 Textbook - Networking from Scratch

## How to Use This Chapter
- Target study time: 14-18 hours.
- Read Chapters 1-9 in order.
- Implement labs while reading, not after finishing.
- Use packet capture for every major concept.

## Learning Outcomes
You should be able to:
- Build TCP and UDP sockets confidently.
- Parse HTTP/1.1 safely and correctly.
- Explain TLS handshake at system level.
- Implement a minimal WebSocket server.
- Diagnose network behavior from packet traces.

---

## Chapter 1 - The Network Stack in Practice

### 1.1 Layered View
```text
Application: HTTP, WebSocket
Transport:   TCP/UDP
Network:     IP
Link:        Ethernet/Wi-Fi
```

### 1.2 Why Layers Matter
Frontend performance issues often originate below application code: retransmissions, handshake delays, congestion, or DNS/TLS overhead.

---

## Chapter 2 - Sockets and I/O Models

### 2.1 Socket Lifecycle (TCP Server)
1. `socket()`
2. `bind()`
3. `listen()`
4. `accept()`
5. `recv()/send()`

### 2.2 Blocking vs Non-Blocking
- Blocking: simpler, fewer states, poor scalability.
- Non-blocking + event loop: scalable, more state complexity.

### 2.3 Diagram: Connection Lifecycle
```text
Client                     Server
SYN      --------------->
         <--------------- SYN-ACK
ACK      --------------->
HTTP req --------------->
HTTP res <---------------
FIN      --------------->
         <--------------- ACK/FIN
ACK      --------------->
```

---

## Chapter 3 - TCP Mechanics You Must Master

### 3.1 Stream Semantics
TCP delivers ordered bytes, not messages. One `recv` may return partial data, exactly one message, or multiple messages.

### 3.2 Flow Control
Receiver window limits sender to prevent buffer overflow.

### 3.3 Congestion Concepts
Sender adjusts behavior based on inferred network congestion. Loss and RTT changes affect throughput and latency.

### 3.4 Engineering Rule
Always design application framing (length-prefix or delimiter). Never rely on packet boundaries.

---

## Chapter 4 - HTTP/1.1 Parsing Correctly

### 4.1 Message Format
- Start line
- Headers
- Blank line
- Optional body

### 4.2 `Content-Length` vs `Transfer-Encoding: chunked`
- `Content-Length`: fixed-size body.
- `chunked`: body sent as variable-size chunks.

### 4.3 Keep-Alive
Reusing connections improves throughput but requires strict parser correctness and request boundary handling.

### 4.4 Diagram: Chunked Body
```text
4\r\nWiki\r\n
5\r\npedia\r\n
0\r\n\r\n
```

### 4.5 Parser Safety Requirements
- header size limits
- line length limits
- timeout limits
- malformed input rejection without crash

---

## Chapter 5 - TLS Handshake (System-Level)

### 5.1 Handshake Steps (Simplified)
1. ClientHello
2. ServerHello + Certificate
3. Key agreement
4. Finished messages
5. Encrypted application data begins

### 5.2 Certificate Validation
- chain of trust
- hostname check
- validity period

### 5.3 Performance Note
TLS adds setup cost; session resumption reduces repeated handshake overhead.

---

## Chapter 6 - WebSocket Protocol Internals

### 6.1 Upgrade Handshake
HTTP request with `Upgrade: websocket`, response `101 Switching Protocols`.

### 6.2 Frame Basics
- FIN
- opcode
- payload length
- mask bit (client-to-server masked)

### 6.3 Control Frames
- Ping/Pong for heartbeat
- Close for graceful shutdown

### 6.4 Diagram: Frame Skeleton
```text
|FIN|OPCODE|MASK|LEN| [extended length] [mask key] [payload]
```

---

## Chapter 7 - Network Debugging Workflow

### 7.1 Trace-First Debugging
When behavior is unclear:
1. Capture traffic.
2. Mark handshake boundaries.
3. Look for retransmissions/timeouts.
4. Correlate with app logs.

### 7.2 Packet Filters (Examples)
- by host
- by port
- by protocol

### 7.3 Latency and Loss Simulation
Use controlled network shaping to test resilience before production.

---

## Chapter 8 - Robust Server Design Patterns

### 8.1 Backpressure
Never assume downstream can always consume at producer speed.

### 8.2 Timeouts
Set explicit read, write, idle, and handshake timeouts.

### 8.3 Error Taxonomy
- client malformed input
- transport reset
- timeout
- resource exhaustion

### 8.4 Observability Fields
Log: connection ID, remote IP, bytes in/out, RTT estimate, protocol stage, close reason.

---

## Chapter 9 - End-to-End Mini Architecture

### 9.1 Build Target
One process exposing:
- `GET /health` (HTTP)
- `/ws` (WebSocket echo)
- structured network logs

### 9.2 Diagram
```text
[Socket acceptor] -> [Protocol dispatcher]
                       |               |
                    [HTTP parser]   [WS frame loop]
```

---

## Quiz A
1. Why is TCP called stream-oriented?
2. Why can one `recv` contain multiple HTTP requests?
3. What does keep-alive improve, and what complexity does it add?
4. What is chunked transfer encoding for?
5. What three checks are mandatory for certificate validation?
6. Why are client WebSocket frames masked?
7. What is backpressure in one sentence?
8. Why are timeouts mandatory in network services?
9. Which is better for low-latency lossy media: TCP or UDP, and why?
10. Why should packet traces be correlated with app logs?

## Quiz A Answers
1. It provides ordered bytes, not message boundaries.
2. TCP coalesces bytes; parser must delimit requests.
3. It reduces connection setup overhead but requires robust boundary parsing/state handling.
4. Sending bodies when final length is unknown at start.
5. trust chain, hostname match, validity period.
6. Protocol safety/intermediary handling requirements.
7. Producer must slow or buffer when consumer is slower.
8. To bound resource usage and avoid hung connections.
9. Usually UDP with app-level handling due to lower overhead/latency sensitivity.
10. Traces show wire truth; logs show app state; together reveal root cause.

---

## Practice Problems (Solved)

### Problem 1: Partial Read Bug
Your parser assumes full request line arrives in one read. Under load it fails.

**Solution**
Implement incremental buffer + state machine parser. Parse only complete tokens, retain remainder.

### Problem 2: Keep-Alive Confusion
Second request on same connection fails unexpectedly.

**Solution**
Likely body boundary parsing bug. Respect `Content-Length` or chunked terminator exactly before reading next request.

### Problem 3: TLS Slow Start
First request latency is high; subsequent requests are lower.

**Solution**
Handshake overhead dominates initial request. Verify session resumption and connection reuse.

### Problem 4: WebSocket Memory Growth
Server memory grows with many slow clients.

**Solution**
Backpressure not enforced. Cap per-connection send queues and apply drop/close strategy.

### Problem 5: Retransmission Spike
Packet capture shows frequent retransmissions.

**Solution**
Investigate network loss/congestion, timeout tuning, MTU issues, or overloaded receiver not reading promptly.

---

## Lab Pack

### Lab 1 - TCP Framing
Build length-prefixed protocol over TCP. Validate with randomized split/coalesced reads.

### Lab 2 - HTTP Parser
Implement request parser with strict limits and keep-alive support.

### Lab 3 - WebSocket Echo
Implement upgrade + frame parser + ping/pong + close handling.

### Lab 4 - Packet Analysis
Capture handshake + request/response + close sequence and annotate each stage.

### Lab 5 - Degraded Network
Run tests under added latency and packet loss. Document throughput/latency changes.

## Completion Checklist
- I can parse HTTP incrementally and safely.
- I can explain each TCP handshake packet in order.
- I can implement and debug WebSocket frame handling.
- I can prove claims with packet evidence.
