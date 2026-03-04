# Tiny HTTP Server Test Plan

## Unit Tests
- Parse request line valid/invalid variants.
- Header parser limits and malformed headers.
- Content-Length handling including mismatch cases.

## Integration Tests
- `GET /health` and `POST /echo` behavior.
- Keep-alive multiple requests over one connection.
- Timeout handling for idle connections.

## Negative Tests
- Oversized headers/body.
- Unsupported methods.
- Truncated requests.
- Invalid CRLF sequence.

## Performance Tests
- Requests/sec and latency on local machine.
- Test profiles:
  - baseline
  - 100 concurrent clients
  - injected latency/loss

## Acceptance Gates
- Zero crashes across all negative tests.
- No request mix-up across keep-alive sequences.
- p95 latency documented for each profile.
