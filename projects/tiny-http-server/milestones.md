# Tiny HTTP Server Milestones

## Milestone 1: TCP Baseline
- Build accept loop and single request/response flow.
- Validate with manual `curl` calls.

Exit criteria:
- Handles 100 sequential requests without crash.

## Milestone 2: Incremental Parser
- Implement state machine parser for request line and headers.
- Add header/body size limits.

Exit criteria:
- 20 malformed request tests pass.

## Milestone 3: Keep-Alive Correctness
- Reuse connection for multiple requests.
- Correctly delimit request bodies.

Exit criteria:
- Back-to-back request tests pass on same socket.

## Milestone 4: Observability + Metrics
- Add structured logging and `/metrics` endpoint.
- Add request latency and error counters.

Exit criteria:
- Metrics update accurately under test load.

## Milestone 5: Load + Failure Validation
- Run load tests under normal and degraded network.
- Capture and analyze failures.

Exit criteria:
- Publish report with baseline vs degraded performance.
