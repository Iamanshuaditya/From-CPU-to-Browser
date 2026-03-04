# Experiment: Cache Effects

## Goal
Observe how memory access patterns change runtime performance due to cache locality.

## Setup
- Implement two data traversal patterns:
  - contiguous array walk
  - pointer-chasing/random access walk
- Use same total element count and workload.

## What to Measure
- runtime duration
- CPU cycles
- cache miss counters
- IPC (instructions per cycle)

## Expected Outcome
Contiguous traversal should show lower cache misses and better throughput.

## Why It Matters
Rendering engines and JS runtimes are heavily affected by memory locality and data layout choices.
