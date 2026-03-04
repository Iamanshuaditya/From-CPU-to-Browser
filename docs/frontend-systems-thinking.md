# Frontend Systems Thinking

Treat frontend performance and reliability as a systems engineering problem, not a component-level styling problem.

## Browser + Server + Network = One System
A user-perceived interaction depends on:
- server response behavior
- network transport and cache path
- browser runtime scheduling
- rendering pipeline and JS execution

Optimizing one layer while ignoring others often moves bottlenecks rather than removing them.

## Real Product Architecture Patterns

### Google Docs-like
- frequent collaboration updates
- local optimistic edits
- conflict resolution + sync pipeline
- strict interaction latency requirements

### Figma-like
- heavy canvas/render workload
- incremental updates and worker offloading
- memory pressure from large documents

### VSCode Web-like
- long-lived sessions
- large state trees and model indexes
- startup + incremental load tradeoffs

## Core Tradeoffs

### Latency vs Throughput
- Latency: time for one action to complete.
- Throughput: total actions handled per time unit.

Example:
Batching can improve throughput but may worsen single-interaction latency if poorly tuned.

### CPU vs Memory
- Caching and memoization reduce CPU, increase memory.
- Aggressive allocation patterns can increase GC pressure.

### Correctness vs Responsiveness
- Strict consistency can block UI.
- Eventual consistency with guardrails can preserve responsiveness.

## How to Reason About Bottlenecks
1. Identify user-facing symptom (slow typing, scroll jank, slow initial load).
2. Map symptom to likely layer.
3. Measure with targeted tools.
4. Form a falsifiable hypothesis.
5. Change one variable.
6. Re-measure and decide.

## Production Slowness Diagnosis Checklist
- Is p95/p99 latency bad or only average?
- Is TTFB or download cost the bottleneck?
- Are long tasks dominating interaction windows?
- Is style/layout invalidation too broad?
- Are there memory growth trends over session time?
- Is hydration cost front-loading too much CPU?
- Are third-party scripts stealing main-thread budget?

## Incident Workflow Template
1. Declare symptom + severity.
2. Capture traces/metrics from impacted path.
3. Build bottleneck matrix by layer.
4. Apply top two fixes with lowest regression risk.
5. Validate before/after in production-like profile.
6. Publish brief postmortem with guardrails.

## Related Content
- [docs/what-happens-when-you-type-a-url.md](what-happens-when-you-type-a-url.md)
- [case-studies/layout-thrashing.md](../case-studies/layout-thrashing.md)
- [case-studies/react-rendering-cost.md](../case-studies/react-rendering-cost.md)
