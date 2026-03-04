# Browser and JS Engine Code Reading Guide

This guide helps you read large production codebases without getting lost.

## Goals
- Build reliable code navigation habits.
- Learn where parser/layout/paint/runtime pieces live.
- Trace one end-to-end path each in Blink and V8.

## Chromium/Blink: Where to Start

### High-level map
- `third_party/blink/renderer/core/html/` -> HTML parsing and DOM-related core logic
- `third_party/blink/renderer/core/css/` -> CSS parsing and style
- `third_party/blink/renderer/core/layout/` -> layout algorithms
- `third_party/blink/renderer/core/paint/` -> paint logic
- `third_party/blink/renderer/platform/graphics/` -> graphics utilities
- `cc/` -> compositor pipeline pieces

### Suggested first trace
1. Find HTML parser entry point in `core/html/parser` area.
2. Trace token -> DOM node creation path.
3. Follow style recalc trigger.
4. Follow layout object update.
5. Locate paint invalidation and compositing handoff.

## V8: Where to Start

### High-level map
- `src/parsing/` -> parser frontend
- `src/interpreter/` -> bytecode interpreter
- `src/heap/` -> memory management and GC
- `src/compiler/` -> optimizing compiler pipeline
- `src/execution/` -> runtime execution integration

### Suggested first trace
1. Parse simple JS function.
2. Find bytecode generation.
3. Observe interpreter dispatch loop.
4. Locate allocation path and GC trigger conditions.

## SpiderMonkey: High-Level Map
- `js/src/frontend/` -> parser/frontend
- `js/src/vm/` -> VM runtime types and execution context
- `js/src/gc/` -> garbage collector implementation
- `js/src/jit/` -> JIT compilers and optimization path

## 7-Day Code Reading Sprint

### Day 1
Set up local clones and build docs for Chromium, V8, SpiderMonkey. Write a one-page directory map.

### Day 2
Blink parser trace: HTML bytes -> tokens -> DOM nodes.

### Day 3
Blink style/layout trace: style invalidation -> layout updates.

### Day 4
V8 parser + interpreter trace for one small script.

### Day 5
V8 heap/GC trace: where allocation and collection policy meet.

### Day 6
SpiderMonkey frontend + GC high-level trace.

### Day 7
Compare architectures and write "design tradeoff notes" (what each engine optimizes for).

## What to Search For
- `Token`, `Parser`, `TreeBuilder`, `StyleRecalc`, `Layout`, `PaintInvalidation`
- `Bytecode`, `Interpreter`, `Heap`, `Marking`, `Scavenger`, `Deopt`
- `TaskRunner`, `Compositor`, `Frame`, `Hydration`

## Common Navigation Tricks
- Start from tests first to discover intended behavior.
- Use `git grep`/`rg` for symbol names from docs and code comments.
- Read call sites before implementation details.
- Trace one happy path completely before exploring edge cases.
- Keep a glossary and map of key classes/functions.

## Code Reading Output Template
For every subsystem, record:
- Entry point file
- Key classes/functions
- Data structures used
- One fast path and one slow path
- One likely performance bottleneck

## Related Content
- [docs/architecture-map.md](architecture-map.md)
- [debugging/perf-debugging.md](../debugging/perf-debugging.md)
- [case-studies/hydration-cost.md](../case-studies/hydration-cost.md)
