# Project: Tiny HTTP Server (C or Rust)

Build a production-style minimal HTTP/1.1 server from scratch.

## Why This Project
This project teaches protocol correctness, backpressure, parsing safety, and network debugging in one focused artifact.

## What You Will Build
- TCP accept loop
- HTTP request parser
- keep-alive handling
- routing for at least 3 endpoints
- structured logging and timing

## Deliverables
- Implementation code (language of your choice)
- parser and integration tests
- latency/throughput measurements
- incident log for 3 injected failures

## Read Before Coding
- [requirements.md](requirements.md)
- [milestones.md](milestones.md)
- [test-plan.md](test-plan.md)
- [debugging-tips.md](debugging-tips.md)
- [rubric.md](rubric.md)

## Quick Start
1. Implement baseline single-threaded blocking server.
2. Add parser state machine with safety limits.
3. Add keep-alive and proper request boundary handling.
4. Add metrics and structured logs.
5. Validate with test plan and rubric.

## Success Criteria
- No parser crashes on malformed inputs.
- Correct handling of partial reads/writes.
- Keep-alive correctness for back-to-back requests.
- Measured behavior under load and packet loss.

## Project to Week Mapping
- Week 1 networking fundamentals
- Week 8 measurement discipline
- Week 9 instrumentation mindset
