# Week 10 Textbook - Hardware Simulation and Constraints

## How to Use This Chapter
- Target study time: 12-16 hours.
- Always define explicit resource budgets first.
- Treat graceful degradation as a feature, not a failure.

## Learning Outcomes
- Profile behavior under tight CPU/memory/network limits.
- Redesign algorithms and data structures for constraints.
- Implement fallback modes with predictable behavior.
- Document tradeoffs using hard metrics.

---

## Chapter 1 - Constraint-Driven Design

### 1.1 Budget Examples
- memory budget (e.g., 64KB class scenarios)
- low CPU frequency
- poor network conditions
- limited graphics acceleration

### 1.2 Prioritization Rule
Preserve correctness and responsiveness of core interactions first.

---

## Chapter 2 - Memory-Constrained Architecture

### 2.1 Allocation Strategy
- compact structs
- pooled allocators
- avoid per-node overhead explosion

### 2.2 Data Lifetime
Track object lifetime classes and reuse buffers aggressively.

### 2.3 Diagram
```text
budget -> reserve core pools -> cap optional caches -> fallback behavior
```

---

## Chapter 3 - CPU-Constrained Scheduling

### 3.1 Frame Budgeting
Cap per-frame work to prevent lockups.

### 3.2 Task Priorities
Urgent user interactions outrank background processing.

### 3.3 Algorithmic Simplification
Use approximate or incremental algorithms where full precision is unnecessary.

---

## Chapter 4 - Render Simplification

### 4.1 Expensive Effects
Blur/shadow/filter-heavy paths are costly on weak GPUs.

### 4.2 Fallback Policy
Disable non-essential effects before sacrificing critical content.

### 4.3 Partial/Tile Rendering
Render only visible/changed regions under pressure.

---

## Chapter 5 - Network Constraint Handling

### 5.1 Slow Network Strategy
- reduce round trips
- compress payloads
- prioritize above-the-fold data

### 5.2 Resilience
Support retry/backoff and offline-aware behavior where possible.

---

## Chapter 6 - Stress Matrix and Failure Modes

### 6.1 Matrix Dimensions
- CPU throttling levels
- memory limits
- packet loss/latency profiles
- update burst intensity

### 6.2 Required Behavior
- no crashes
- bounded memory
- deterministic fallback
- clear telemetry

---

## Quiz A
1. Why define budgets before optimization?
2. Why can caches become liabilities under tight memory?
3. What should be degraded first in UI quality mode?
4. Why cap frame work under CPU pressure?
5. Why measure combined constraints, not isolated ones?
6. What is a good memory-pressure signal?
7. Why might approximate algorithms be acceptable?
8. What is the value of deterministic fallback behavior?

## Quiz A Answers
1. Budgets define success criteria and prevent unbounded scope.
2. Cache memory can crowd out critical runtime data.
3. Non-essential visual effects before core functionality.
4. To maintain responsiveness and avoid long blocking tasks.
5. Real systems experience compound bottlenecks.
6. Sustained high allocation + frequent GC or OOM proximity.
7. They trade precision for latency/resource stability.
8. Predictable user experience and easier debugging.

---

## Practice Problems (Solved)

### Problem 1: OOM Under Load
Memory spikes during large page render.

**Solution**
Introduce object pooling, cap caches, and stream/segment render work.

### Problem 2: Frozen UI on Low-End Device
Input blocked during heavy computations.

**Solution**
Chunk tasks, prioritize input lane, and defer non-critical updates.

### Problem 3: Degraded Network Breaks UX
Critical data arrives late and blank screen persists.

**Solution**
Show skeleton/fallback early, prioritize critical payload, and progressively enhance.

---

## Lab Pack

### Lab 1 - Memory Budget Drill
Run with strict memory cap; identify top consumers and reduce footprint.

### Lab 2 - CPU Throttle Drill
Profile frame-time violations at multiple throttle levels.

### Lab 3 - Network Stress Drill
Test 2G/3G + loss profile and document UX impact.

### Lab 4 - Graceful Degradation Mode
Implement low-resource mode and compare usability/perf metrics.

## Completion Checklist
- Core interactions remain usable under constraints.
- Resource usage is bounded and measurable.
- Degradation policy is explicit and testable.
