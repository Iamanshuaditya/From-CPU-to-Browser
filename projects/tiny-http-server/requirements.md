# Tiny HTTP Server Requirements

## Functional Requirements
- Accept TCP connections on configurable host/port.
- Parse HTTP/1.1 request line and headers incrementally.
- Support methods: `GET` and `POST`.
- Support paths:
  - `/health` -> `200 OK`
  - `/echo` -> echoes request body
  - `/metrics` -> simple counters/timings
- Support keep-alive with correct boundary handling.
- Return valid status codes for parse errors and unsupported routes.

## Non-Functional Requirements
- Max header bytes limit (document exact value).
- Max request body bytes limit (document exact value).
- Read timeout and idle timeout.
- Structured logs per request:
  - request id
  - method/path/status
  - bytes in/out
  - latency ms

## Reliability Requirements
- Handle malformed headers without crash.
- Handle partial reads and partial writes.
- Gracefully close broken connections.
- Prevent unbounded memory growth under malicious input.

## Observability Requirements
- Count total requests, errors, active connections.
- Expose p50/p95/p99 latency from in-process histograms (simple bins allowed).
- Record parse error categories.

## Stretch Goals
- Pipelining support.
- Optional thread pool or async runtime.
- Chunked transfer encoding response support.
