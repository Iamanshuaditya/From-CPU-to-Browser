# Week 6 Textbook - Virtual DOM and Reconciliation

## How to Use This Chapter
- Target study time: 12-16 hours.
- Build the reconciler incrementally.
- Measure every optimization; do not trust intuition.

## Learning Outcomes
- Implement O(n)-style practical diff heuristics.
- Handle keyed list reconciliation correctly.
- Build batching/scheduling for smoother frames.
- Detect and reduce layout thrashing.

---

## Chapter 1 - Why Virtual DOM Exists

### 1.1 Direct DOM Cost Model
Frequent direct DOM mutations can trigger repeated style/layout/paint work and become hard to coordinate.

### 1.2 Virtual Representation
Virtual tree allows deterministic diffing and batched commits.

### 1.3 Diagram
```text
state change -> virtual tree render -> diff -> patch list -> DOM commit
```

---

## Chapter 2 - Diffing Heuristics

### 2.1 Complexity Insight
General tree edit distance is too expensive; UI frameworks assume common patterns to achieve near O(n).

### 2.2 Core Heuristics
- different node type => replace subtree
- same type => compare props and recurse children
- keyed children => preserve identity and reorder intelligently

---

## Chapter 3 - Keyed Reconciliation

### 3.1 Why Keys Matter
Keys map logical identity across renders.

### 3.2 Failure of Index Keys
Reordered lists with index keys can cause unnecessary moves or state corruption.

### 3.3 Diagram
```text
Old: [A(k1), B(k2), C(k3)]
New: [C(k3), A(k1), B(k2)]
```
Keyed reconciliation should move nodes, not recreate all.

---

## Chapter 4 - Patch Generation and Commit

### 4.1 Patch Types
- create/remove node
- set/remove property
- set text
- move child

### 4.2 Two-Phase Model
- render/diff phase: pure, no DOM side effects
- commit phase: apply mutations atomically

---

## Chapter 5 - Batching and Scheduling

### 5.1 Batching
Multiple state updates in a tick should coalesce into fewer commits.

### 5.2 Priority Scheduling
- high: input-driven updates
- medium: visible content updates
- low: background updates

### 5.3 Time Slicing
Yield periodically to keep input latency low.

---

## Chapter 6 - Reflow/Repaint Control

### 6.1 Layout Thrashing Pattern
Repeated read/write/read/write cycles force synchronous layout.

### 6.2 FastDOM Pattern
- batch all reads
- batch all writes

### 6.3 Composite-Only Updates
Prefer transform/opacity when possible for animation paths.

---

## Chapter 7 - Instrumentation

Track per update:
- render time
- diff time
- commit time
- DOM op count
- forced reflow count
- dropped frame count

---

## Quiz A
1. Why is full tree replacement usually unacceptable?
2. What problem do keys solve?
3. Why separate diff from commit?
4. How does batching improve performance?
5. What causes layout thrashing?
6. Why can index keys be dangerous?
7. What is a practical signal of over-committing DOM ops?
8. Why prioritize input updates?

## Quiz A Answers
1. Too many DOM ops and layout/paint invalidations.
2. Stable identity mapping across renders.
3. Purity, interruptibility, and safer side-effect handling.
4. Reduces repeated pipeline work and commit frequency.
5. Alternating reads and writes that force sync layout recalculation.
6. Reorders break identity assumptions, causing incorrect reuse/state issues.
7. High commit time and frame drops under moderate load.
8. To preserve responsiveness and user-perceived smoothness.

---

## Practice Problems (Solved)

### Problem 1: Reordering Bug
List reorder causes checkbox state to jump rows.

**Solution**
Keys are unstable (likely index-based). Use stable item identity keys.

### Problem 2: Slow Commits
Commit phase dominates update time.

**Solution**
Reduce patch count, avoid unnecessary node replacements, and batch mutations.

### Problem 3: Scroll Jank
Animation stutters while measuring layout every frame.

**Solution**
Move layout reads outside write-heavy loops; use cached measurements and RAF batching.

---

## Lab Pack

### Lab 1 - Diff Correctness Suite
Run tree-diff edge cases including nested replacements and keyed reorders.

### Lab 2 - 10k Node Stress Test
Measure update latency and dropped frames.

### Lab 3 - Scheduler Priority Test
Simulate high-frequency input + background updates.

### Lab 4 - Layout Thrash Detector
Instrument forced reflows and validate reduction after batching.

## Completion Checklist
- Keyed reconciliation is correct under reorder.
- Commit phase is measurably bounded.
- Frame-time behavior is stable under stress.
