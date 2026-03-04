# Master Progress Checklist

Use this as the single source of truth for progress.

## Weekly Completion

### Week 0.5
- [ ] Read all chapters
- [ ] Complete quiz and practice problems
- [ ] Finish all labs
- [ ] Submit benchmark report

### Week 1
- [ ] Read all chapters
- [ ] Build tiny HTTP server baseline
- [ ] Complete packet capture analysis
- [ ] Finish quiz and labs

### Weeks 2-3
- [ ] Build HTML tokenizer + tree builder
- [ ] Implement CSS parser + specificity
- [ ] Implement layout subset
- [ ] Implement paint/composite subset
- [ ] Collect stage timing metrics

### Weeks 4-5
- [ ] Implement lexer/parser/AST
- [ ] Implement bytecode VM
- [ ] Implement closures/prototypes
- [ ] Implement GC baseline
- [ ] Pass event loop ordering tests

### Week 6
- [ ] Implement VDOM diff and commit
- [ ] Implement keyed reconciliation
- [ ] Add scheduling and batching
- [ ] Run 10k-node stress test

### Week 7
- [ ] Implement mini Fiber work loop
- [ ] Implement core hooks
- [ ] Implement SSR/hydration prototype
- [ ] Document render vs commit behavior

### Week 8
- [ ] Capture performance baseline
- [ ] Run optimization passes
- [ ] Publish before/after metrics
- [ ] Document tradeoffs and regressions

### Week 9
- [ ] Add engine instrumentation
- [ ] Analyze GC/JIT/layout traces
- [ ] Run JS/WASM boundary experiment
- [ ] Submit findings report

### Week 10
- [ ] Define resource budgets
- [ ] Implement constrained mode
- [ ] Run stress matrix tests
- [ ] Document degradation policy

### Weeks 11-12
- [ ] Integrate all subsystems
- [ ] Stabilize correctness
- [ ] Complete optimization and hardening
- [ ] Deliver final demo + docs

## Project Completion
- [ ] [Tiny HTTP Server](../projects/tiny-http-server/README.md)
- [ ] [HTML+CSS Parser + Tiny Layout Engine](../projects/html-css-renderer/README.md)
- [ ] [Mini Browser Pipeline](../projects/mini-browser/README.md)
- [ ] [Mini JavaScript Runtime](../projects/mini-js-runtime/README.md)
- [ ] [Mini React](../projects/mini-react/README.md)

## Experiments Completion
- [ ] [Cache Effects](../experiments/cache-effects/README.md)
- [ ] [Event Loop Order](../experiments/event-loop-order/README.md)
- [ ] [Layout Cost](../experiments/layout-cost/README.md)
- [ ] [DOM Size Impact](../experiments/dom-size-impact/README.md)
- [ ] [HTTP/1 vs HTTP/2](../experiments/http1-vs-http2/README.md)
- [ ] [GC Pressure](../experiments/gc-pressure/README.md)

## Case Studies Completion
- [ ] [Layout Thrashing](../case-studies/layout-thrashing.md)
- [ ] [Hydration Cost](../case-studies/hydration-cost.md)
- [ ] [React Rendering Cost](../case-studies/react-rendering-cost.md)

## Minimum Completion vs Mastery Completion

### Minimum Completion
- Complete all weekly quizzes and labs.
- Complete at least 3/5 projects.
- Complete at least 3/6 experiments.
- Complete all three case studies.
- Produce one measurable performance report.

### Mastery Completion
- Complete all projects with rubric score >= 85%.
- Complete all experiments with reproducible measurements.
- Complete all challenges at medium and at least 2 hard challenges.
- Submit capstone with full documentation and reproducible benchmarks.
