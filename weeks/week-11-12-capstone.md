# Weeks 11-12 Textbook - Capstone Integration and Delivery

## How to Use This Chapter
- Target study time: 28-36 hours across two weeks.
- Week 11: integration and correctness stabilization.
- Week 12: optimization, hardening, documentation, final demo.

## Capstone Goal
Deliver a complete system proving end-to-end understanding from CPU constraints to browser rendering and modern frontend runtime behavior.

## Required Subsystems
1. Browser engine core (HTML/CSS/layout/paint/composite)
2. JavaScript runtime (execution + GC + event loop)
3. React-like UI runtime (reconciliation + hooks + scheduling)
4. SSR streaming + hydration path
5. Profiling/observability stack
6. WASM optimization path
7. Constraint-mode validation

---

## Chapter 1 - Architecture Freeze and Interfaces

### 1.1 Interface Contracts
Define clear boundaries:
- parser output schema
- style/layout tree interfaces
- DOM/runtime bridge API
- metrics event schema

### 1.2 Diagram
```text
Engine Core <-> JS Runtime <-> UI Runtime <-> SSR Layer
      \----------------------------------------------/
                    Metrics/Tracing Layer
```

### 1.3 Freeze Rule
Do not expand scope after architecture freeze unless blocking defect requires it.

---

## Chapter 2 - Week 11 Integration Plan

### Day 1-2
- connect browser engine + JS runtime
- verify event dispatch and DOM mutation path

### Day 3-4
- integrate React-like runtime
- validate state update correctness and commit behavior

### Day 5
- integrate SSR streaming and hydration
- add mismatch diagnostics

### Day 6-7
- run correctness test suite
- triage and fix highest-severity defects

---

## Chapter 3 - Week 12 Optimization and Hardening

### Day 8
capture full baseline metrics and top bottlenecks.

### Day 9-10
apply highest-impact optimizations (render path, interaction latency).

### Day 11
integrate and evaluate WASM acceleration path.

### Day 12
run constrained hardware/network matrix.

### Day 13
finalize docs, architecture diagrams, benchmark tables.

### Day 14
full dry run from clean build to demo.

---

## Chapter 4 - Benchmark and Validation Protocol

### 4.1 Baseline + Final Metrics
- cold/warm start times
- frame-time percentiles
- GC pause distribution
- memory over time
- network constrained performance

### 4.2 Reproducibility Requirements
- fixed test scripts
- fixed profiles (CPU/network)
- repeated runs and median/p95 reporting

---

## Chapter 5 - Reliability and Failure Handling

### 5.1 Must-Have Reliability Behaviors
- graceful parser error recovery
- runtime exception containment
- bounded retry policies
- no silent failure paths

### 5.2 Failure-Mode Docs
For each failure class:
- detection signal
- user impact
- mitigation/fallback
- long-term fix strategy

---

## Chapter 6 - Final Demo Blueprint

### 6.1 Demo Sequence
1. architecture walk-through
2. correctness demo
3. performance baseline
4. optimization gains
5. constrained-mode behavior
6. known limitations + next steps

### 6.2 Submission Package
- source and build scripts
- reproducibility guide
- architecture report
- performance report
- failure-mode report

---

## Quiz A
1. Why freeze architecture before late-stage optimization?
2. What is the risk of optimizing before correctness?
3. Why include p95 metrics, not just averages?
4. Why run a clean-environment dry run?
5. Why require explicit failure-mode documentation?
6. What makes a benchmark suite credible?
7. Why can WASM optimization be net-negative?
8. Why separate integration week from optimization week?

## Quiz A Answers
1. Prevents scope churn and unstable interfaces.
2. You may speed up wrong or broken behavior.
3. User pain often appears in tail latency.
4. Ensures reproducibility and catches hidden env dependencies.
5. Reliability depends on known, testable failure handling.
6. Controlled conditions, repeated runs, transparent methodology.
7. Boundary overhead can outweigh compute speed gains.
8. Stabilized correctness is a prerequisite for meaningful optimization.

---

## Practice Problems (Solved)

### Problem 1: Integration Deadlock
UI update path waits on runtime lock while runtime waits on render callback.

**Solution**
Redesign lock boundaries and event ordering; avoid cyclic waiting between layers.

### Problem 2: Great Average, Bad p95
Average frame time is fine but p95 spikes heavily.

**Solution**
Identify tail events (GC, large layout invalidations, sync I/O) and target those specifically.

### Problem 3: Demo Non-Reproducible
Works on dev machine but fails on clean setup.

**Solution**
Pin dependencies/toolchain, script setup, and verify from clean environment before release.

---

## Capstone Lab Pack

### Lab 1 - End-to-End Integration Test Harness
Automate key user flows and correctness assertions across all layers.

### Lab 2 - Full Trace Capture
Capture and annotate one end-to-end interaction across parse/style/layout/script/composite phases.

### Lab 3 - Optimization Sprint
Select top two bottlenecks, apply fixes, document measured gains.

### Lab 4 - Constraint Validation
Run stress matrix and verify fallback mode behavior.

## Final Completion Checklist
- All subsystem contracts are documented.
- Benchmarks are reproducible and honest.
- Reliability/failure handling is explicit.
- Final demo proves understanding from low-level mechanisms to UX outcomes.
