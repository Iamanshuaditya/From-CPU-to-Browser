# Week 9 Textbook - Engine Modification and Debugging

## How to Use This Chapter
- Target study time: 14-18 hours.
- Keep patches small and measurable.
- Prefer trace-driven debugging over intuition.

## Learning Outcomes
- Build and run large engines with custom instrumentation.
- Add targeted tracing in JS and browser engine paths.
- Analyze GC/JIT/layout/compositor behavior from data.
- Evaluate JS/WASM boundary performance realistically.

---

## Chapter 1 - Working Safely in Large Codebases

### 1.1 Build Discipline
- pin toolchain versions
- keep reproducible build notes
- isolate feature flags for instrumentation

### 1.2 Patch Strategy
Small, scoped commits with one hypothesis each.

---

## Chapter 2 - JavaScript Engine Instrumentation

### 2.1 Interpreter Hooks
Measure opcode frequency and per-op latency buckets.

### 2.2 JIT Decision Tracing
Track optimize/deopt events and reasons.

### 2.3 GC Telemetry
Emit pause times, reclaimed bytes, promotion rate, root scan cost.

### 2.4 Diagram
```text
source -> bytecode -> interp/JIT -> heap -> GC events -> telemetry sink
```

---

## Chapter 3 - Browser Engine Tracing

### 3.1 Layout Tracing
Capture invalidation source, subtree size, and per-pass time.

### 3.2 Paint/Composite Tracing
Track paint command count, layer count, raster cost, composite duration.

### 3.3 Multi-Thread Interaction
Correlate main-thread and compositor-thread timelines.

---

## Chapter 4 - High-Signal Diagnostics

### 4.1 What to Log
- event timestamps
- object sizes/counts
- stage identifiers
- correlation IDs

### 4.2 What Not to Log
High-volume low-value logs that perturb timing.

### 4.3 Sampling vs Full Tracing
Use sampling for low overhead; full tracing for targeted deep dives.

---

## Chapter 5 - WebAssembly Boundary Optimization

### 5.1 Good WASM Candidates
High arithmetic intensity, predictable memory access.

### 5.2 Boundary Costs
- call overhead
- memory conversion/copies
- synchronization overhead

### 5.3 Optimization Pattern
Batch calls and operate on shared typed-array buffers.

### 5.4 Diagram
```text
JS orchestration -> WASM bulk kernel -> JS presentation
```

---

## Chapter 6 - Experimental Method

### 6.1 Hypothesis Loop
1. baseline measurement
2. targeted patch
3. controlled rerun
4. compare with confidence

### 6.2 Regression Guardrails
Keep performance/unit tests to detect instrumentation side effects.

---

## Quiz A
1. Why keep instrumentation behind flags?
2. What makes a deopt reason valuable?
3. Why correlate layout traces with compositor traces?
4. Why can too much logging hide real behavior?
5. When does WASM fail to provide end-to-end gains?
6. Why use correlation IDs in traces?
7. What metric best reflects GC user impact?
8. Why are small patch increments preferred?

## Quiz A Answers
1. To minimize overhead and isolate behavior changes.
2. It reveals violated optimization assumptions.
3. UI jank can emerge from cross-thread pipeline interactions.
4. Logging overhead perturbs timings and signal quality.
5. When boundary overhead or I/O dominates compute cost.
6. To stitch related events across subsystems.
7. Pause duration and frequency under real workload.
8. Easier attribution, rollback, and debugging.

---

## Practice Problems (Solved)

### Problem 1: JIT Thrash
Function repeatedly optimizes and deoptimizes.

**Solution**
Inspect deopt reasons; stabilize shapes/types or disable speculative path for that hotspot.

### Problem 2: Layout Jank Unclear
Main thread appears fine but frame drops persist.

**Solution**
Check compositor/raster traces for bottlenecks and synchronization gaps.

### Problem 3: WASM Port No Gain
Kernel is faster in isolation but app latency unchanged.

**Solution**
Boundary marshalling and frequent calls dominate. Batch operations and reduce crossings.

---

## Lab Pack

### Lab 1 - Opcode Histogram
Add interpreter counters and identify top 10 opcodes by frequency.

### Lab 2 - GC Timeline
Collect and visualize GC events over a stress workload.

### Lab 3 - Layout Invalidation Trace
Patch engine to log invalidation causes and subtree sizes.

### Lab 4 - JS/WASM Boundary Benchmark
Compare many small calls vs few batched calls.

## Completion Checklist
- Instrumentation produces actionable, low-noise signals.
- At least one bottleneck is proven and improved with data.
