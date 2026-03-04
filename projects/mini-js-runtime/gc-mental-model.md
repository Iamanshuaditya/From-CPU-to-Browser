# GC Mental Model

## Core Idea
Garbage collection reclaims objects unreachable from roots.

## Root Set
Typical roots include:
- global scope
- stack frames
- closure environments
- runtime internals

## Mark-and-Sweep Model
1. Mark reachable objects from roots.
2. Sweep unmarked objects as garbage.

## Common Leak Patterns
- retaining references in global maps
- listener lists never cleaned up
- closure captures of large object graphs

## Debugging Workflow
1. Observe memory growth trend.
2. Capture snapshots at intervals.
3. Compare retained object paths.
4. Remove one retention path and re-measure.

## Engine Pitfalls (C/C++/Rust host code)
- missing root marking for internal handles
- stale pointers to moved/freed objects
- delayed cleanup in long-lived registries
