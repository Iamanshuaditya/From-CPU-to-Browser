# Week 0.5 Textbook - CPU and Memory Architecture

## How to Use This Chapter
- Target study time: 12-16 hours.
- Read Chapters 1-8 in order.
- Do quizzes without notes first.
- Do practice problems after quizzes.
- Compare with answer key and rewrite weak areas.

## Learning Outcomes
By the end of this week, you should be able to:
- Explain modern CPU execution flow beyond the simplified model.
- Reason about latency vs throughput in instruction pipelines.
- Diagnose branch misprediction and cache-miss bottlenecks.
- Explain virtual memory, TLB behavior, and page faults.
- Measure low-level effects with real profiling tools.

---

## Chapter 1 - Mental Model: From Source Code to Silicon

### 1.1 The Full Journey
Your C/C++/Rust code flows through these layers:
1. Source code
2. Compiler frontend and optimizer
3. Machine code (ISA instructions)
4. CPU decode to micro-ops
5. Execution units + memory hierarchy

### 1.2 ISA vs Microarchitecture
- ISA is the software-visible contract.
- Microarchitecture is the hardware implementation.
- Same ISA, different microarchitecture => different performance.

### 1.3 Diagram: End-to-End Path
```text
Source -> Compiler -> Machine Code -> Fetch -> Decode -> Execute -> Retire
                                              |        |         |
                                           I-Cache   uOps    ALU/FPU/LoadStore
```

### 1.4 Why This Matters
When frontend apps stutter, root causes can begin here: poor branch predictability, cache-unfriendly data layout, or syscall overhead.

---

## Chapter 2 - Pipeline and Instruction-Level Parallelism

### 2.1 Pipeline Stages (Practical View)
- Fetch: pull instruction bytes from instruction cache.
- Decode: translate bytes into internal operations.
- Dispatch/Schedule: assign operations to execution units.
- Execute: perform math, load/store, branch checks.
- Retire: commit architecturally visible results in order.

### 2.2 Dependencies
- RAW (Read-after-write): true dependency.
- WAR/WAW: scheduling hazards in out-of-order cores.
- Control dependency: branch outcome controls next instruction path.

### 2.3 Latency vs Throughput
- Latency: time for one instruction to produce result.
- Throughput: how many such ops/cycle sustained.
- A loop can be latency-bound or throughput-bound.

### 2.4 Diagram: Dependency Chain vs Independent Ops
```text
Chain (slow to overlap):
A -> B -> C -> D

Independent (good overlap):
A1  A2  A3  A4
B1  B2  B3  B4
```

### 2.5 Common Optimization Insight
Breaking long dependency chains often gives bigger wins than micro-optimizing individual instructions.

---

## Chapter 3 - Branch Prediction and Control Flow Cost

### 3.1 Why Branch Prediction Exists
The CPU must guess branch direction before the condition is fully resolved to keep the pipeline busy.

### 3.2 Correct vs Incorrect Prediction
- Correct: pipeline stays full.
- Incorrect: speculative work discarded; pipeline flushed.

### 3.3 Diagram: Misprediction Penalty
```text
Cycle: 1 2 3 4 5 6 7 8
Guess: T T T T
Actual at cycle 5: F
Result: flush at 5, restart fetch at correct target
```

### 3.4 Patterns That Hurt
- Randomized `if` condition in hot loops.
- Data-dependent branches with 50/50 behavior.
- Unsorted data causing unpredictable branch history.

### 3.5 Patterns That Help
- Branchless transforms for tiny hot loops.
- Data bucketing/sorting to improve predictability.
- Early exits only when highly biased outcomes exist.

---

## Chapter 4 - Cache Hierarchy and Data Locality

### 4.1 Memory Is Not Flat
Approximate cost tiers (conceptual):
- Registers: fastest
- L1 cache
- L2 cache
- L3 cache
- DRAM: much slower

### 4.2 Cache Lines
Memory moves in cache-line chunks (commonly 64 bytes). Accessing one byte often fetches its neighbors too.

### 4.3 Locality Types
- Spatial locality: nearby addresses reused.
- Temporal locality: same address reused soon.

### 4.4 Diagram: Stride Access Impact
```text
Contiguous:  [x][x][x][x][x][x][x][x]  (good locality)
Large stride:[x][ ][ ][ ][x][ ][ ][ ]  (worse locality)
```

### 4.5 Data Layout Tradeoff
- Array of Structs (AoS): good when reading full objects.
- Struct of Arrays (SoA): better for vectorized/single-field scans.

### 4.6 False Sharing (Multithreaded)
Two threads updating different variables on the same cache line force coherence traffic and slow each other down.

---

## Chapter 5 - Virtual Memory, TLB, and Page Faults

### 5.1 Virtual Address Space
Each process sees a virtual address map. MMU + page tables translate to physical memory.

### 5.2 TLB Role
TLB caches virtual-to-physical translations. TLB misses trigger expensive page-table walks.

### 5.3 Page Fault Types
- Minor page fault: mapping established without disk I/O.
- Major page fault: disk access required, very expensive.

### 5.4 Diagram: Address Translation
```text
Virtual Address -> TLB lookup -> (hit) Physical Address
                        |
                       miss
                        v
                 Page table walk -> fill TLB -> continue
```

### 5.5 Practical Effect
Workloads with large random memory footprints can become translation-bound even if raw bandwidth seems available.

---

## Chapter 6 - Process Memory Layout and Syscalls

### 6.1 Typical Process Layout
- `.text`: executable instructions
- `.data/.bss`: global/static storage
- heap: dynamic allocations
- stack: call frames and locals

### 6.2 System Call Boundary
Syscalls transition from user mode to kernel mode for privileged services.

### 6.3 Why Excess Syscalls Hurt
- Privilege transitions
- Kernel validation/bookkeeping
- Potential context-switch and cache disruption

### 6.4 Diagram: User-Kernel Transition
```text
User code -> syscall instruction -> kernel handler -> return to user
```

### 6.5 Engineering Pattern
Batch I/O and reduce syscall frequency in hot paths.

---

## Chapter 7 - Measuring Reality (Tool-Driven)

### 7.1 Core Tools
- `objdump` or `llvm-objdump` for disassembly.
- `perf stat` for high-level hardware counters.
- `perf record/report` for hotspot attribution.
- `gdb` or `lldb` for stack/memory validation.

### 7.2 Minimal Counter Set
Start with:
- cycles
- instructions
- branches
- branch-misses
- cache-misses

### 7.3 IPC Interpretation
IPC = instructions per cycle.
- Low IPC with high cache misses => memory bottleneck.
- Low IPC with high branch misses => control-flow bottleneck.

---

## Chapter 8 - Applied Case Study: Why a Loop Is Slow

### 8.1 Scenario
You profile a hot loop that filters data and updates counters. CPU usage is high but throughput is low.

### 8.2 Investigation Path
1. Check branch-miss ratio.
2. Check cache-miss ratio.
3. Inspect disassembly for dependency chains.
4. Try data reordering and branchless variant.
5. Re-measure with same dataset and pinned settings.

### 8.3 Expected Report Structure
- Baseline counters + timing
- Hypothesis
- Change made
- Counter deltas
- Conclusion

---

## Quiz A (Concepts)
1. Why can two CPUs with the same ISA show different performance on the same binary?
2. What is a pipeline flush and what commonly triggers it?
3. Distinguish latency-bound and throughput-bound loops.
4. Why do cache lines matter for algorithm performance?
5. What is the difference between a TLB miss and a page fault?
6. Why can many small syscalls harm throughput?
7. Explain false sharing in one sentence.
8. Why is contiguous iteration usually faster than pointer chasing?
9. What does high branch-miss ratio usually indicate?
10. Why should performance claims always include counters, not just elapsed time?

## Quiz A Answer Key
1. Microarchitecture differences (pipeline, cache, predictor, scheduler) change real performance.
2. Discarding speculative pipeline work, often due to branch misprediction.
3. Latency-bound depends on dependency chain delay; throughput-bound on unit saturation rate.
4. Data loads occur per line; good locality reduces misses and memory stalls.
5. TLB miss triggers translation lookup; page fault means mapping not ready (possibly disk I/O).
6. Frequent user-kernel transitions and overhead per call.
7. Different-thread writes on same cache line causing coherence invalidations.
8. Better spatial locality and prefetch friendliness.
9. Unpredictable control flow.
10. Time alone hides cause; counters reveal mechanism.

---

## Practice Problems (With Solutions)

### Problem 1: Branch Behavior
You have a loop with `if (arr[i] > threshold)` on random data.
- Predict branch miss behavior.
- Suggest one transformation.

**Solution**
- Branch miss likely high due to low predictability.
- Use branchless accumulation (e.g., arithmetic mask) or data bucketing to improve predictability.

### Problem 2: Locality
Two versions sum values:
- Version A iterates contiguous array.
- Version B follows linked-list pointers.
Explain expected counter differences.

**Solution**
- Version A should show fewer cache and TLB misses.
- Version B likely has higher misses and lower IPC due to random memory access.

### Problem 3: Syscall Cost
A logger writes one line at a time with separate `write` calls.
How to improve?

**Solution**
- Buffer logs and write in chunks.
- Fewer syscalls reduce mode-switch overhead and improve throughput.

### Problem 4: Register Pressure
A loop introduces many temporaries after refactor and slows down.
Why?

**Solution**
- Register pressure can cause spills to stack, increasing memory traffic and latency.

### Problem 5: Root Cause Ranking
Profile shows: high branch misses, moderate cache misses, low syscall count.
What to optimize first?

**Solution**
- Start with branch predictability/control-flow simplification since branch misses are dominant.

---

## Lab Assignment Pack

### Lab 1 - Source to Assembly Mapping
- Write a 30-50 line benchmark loop.
- Compile `-O0` and `-O2/-O3`.
- Annotate at least 10 assembly instructions with source meaning.

### Lab 2 - Branch Predictor Experiment
- Build predictable and unpredictable branch datasets.
- Record runtime + branch miss counters.
- Create one plot/table and explain variance.

### Lab 3 - Cache Locality Experiment
- Compare contiguous vs pointer-chasing access.
- Measure time + cache misses.
- Explain using cache-line locality and prefetch behavior.

### Lab 4 - Syscall Batching Experiment
- Compare per-item writes vs buffered writes.
- Measure wall time and syscall counts.
- Discuss tradeoffs (latency vs throughput).

## Completion Checklist
- I can explain every counter I collect.
- I can connect each optimization to a hardware mechanism.
- I can reproduce benchmark results consistently.
- I can defend my conclusions with evidence.
