# Week 8 Textbook - Performance Engineering

## How to Use This Chapter
- Target study time: 12-16 hours.
- Treat every claim as a measurement task.
- Keep a single benchmark protocol for consistency.

## Learning Outcomes
- Build reproducible performance baselines.
- Diagnose bottlenecks across CPU, rendering, and network.
- Prioritize changes by user impact metrics.
- Deliver evidence-based optimization reports.

---

## Chapter 1 - Measurement Framework

### 1.1 Baseline First
Capture before optimization:
- LCP, CLS, INP/FID
- JS execution time
- layout/paint timings
- bundle size and request waterfall

### 1.2 Reproducibility
Fix test conditions: device profile, throttling, network profile, cache state.

---

## Chapter 2 - Profiling Tools

### 2.1 Performance Trace
Use flame chart to find long tasks and expensive phases.

### 2.2 React Profiler
Locate over-rendering and expensive component trees.

### 2.3 Memory Profiling
Detect leaks, detached DOM, and allocation hotspots.

---

## Chapter 3 - Core Web Vitals Mechanisms

### 3.1 LCP
Primarily influenced by server latency, critical resource loading, and render path.

### 3.2 CLS
Driven by layout shifts from unsized media, dynamic inserts, or unstable fonts.

### 3.3 INP/FID
Driven by main-thread blocking and long JS tasks.

---

## Chapter 4 - Optimization Strategy by Layer

### 4.1 Rendering
- memoization where stable
- virtualization for long lists
- reduce unnecessary updates

### 4.2 Network/Assets
- responsive images
- lazy loading
- smarter preload/prefetch

### 4.3 Bundling
- route/component splitting
- dead-code elimination
- defer low-priority scripts

### 4.4 Fonts
- subset and preload critical
- choose `font-display` intentionally

---

## Chapter 5 - Critical Rendering Path

### 5.1 Principle
Prioritize only resources required for first meaningful paint.

### 5.2 Diagram
```text
HTML parse -> critical CSS -> render tree -> layout -> paint
```
Delay non-critical scripts/styles until after first render path.

---

## Chapter 6 - Optimization Validation

### 6.1 Good Report Structure
- baseline table
- change list with rationale
- per-change metric deltas
- regressions and reversions

### 6.2 Statistical Thinking
Use repeated runs and percentile comparisons, not single-run screenshots.

---

## Quiz A
1. Why is baseline capture mandatory?
2. Why can average metrics hide user pain?
3. What usually impacts LCP most?
4. Why can memoization hurt performance sometimes?
5. What causes CLS spikes?
6. Why can too many preloads degrade performance?
7. What indicates interaction jank in traces?
8. Why isolate one optimization at a time?

## Quiz A Answers
1. Without baseline, improvement claims are ungrounded.
2. Tail latency and outliers are hidden by averages.
3. Critical resource path and server/render delays.
4. Dependency tracking and cache overhead can exceed saved work.
5. Unsized assets and late layout-changing content.
6. Bandwidth contention for truly critical resources.
7. Long main-thread tasks around input handling.
8. To attribute impact and avoid confounded results.

---

## Practice Problems (Solved)

### Problem 1: LCP Stuck Above 3s
Hero image loads late despite smaller JS bundle.

**Solution**
Prioritize hero image (proper preload/priority), optimize format/size, and reduce server/TTFB path.

### Problem 2: CLS Regression After Ads
Layout jumps after ad slot loads.

**Solution**
Reserve fixed/aspect-ratio space for ad containers.

### Problem 3: Input Lag During Filtering
Typing causes heavy synchronous computation.

**Solution**
Chunk work, debounce where valid, and move non-urgent updates to lower priority.

---

## Lab Pack

### Lab 1 - Baseline Dashboard
Create a metrics table for 10 repeated runs.

### Lab 2 - Rendering Optimization
Apply list virtualization and compare frame metrics.

### Lab 3 - Asset Optimization
Improve image/font strategy and measure LCP/CLS shift.

### Lab 4 - Bundle Optimization
Remove dead code and split routes; verify behavior.

## Completion Checklist
- Each optimization is backed by reproducible numbers.
- Final report explains both wins and remaining bottlenecks.
