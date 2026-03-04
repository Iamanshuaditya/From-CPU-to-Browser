# Challenge: Implement JS Garbage Collector

**Difficulty:** hard

## Objective
Implement mark-and-sweep GC in the mini JS runtime with clear root management.

## Scope
- root set discovery
- mark traversal
- sweep and reclamation
- basic metrics output

## Acceptance Criteria
- [ ] Correctly reclaims unreachable objects.
- [ ] Preserves reachable objects in closure/prototype graphs.
- [ ] Includes stress tests for churn and cyclic references.
- [ ] Emits pause and reclaimed-object metrics.
- [ ] Documents known limitations and safety assumptions.

## Submission Requirements
- design doc for object model and roots
- test suite output
- before/after memory trend chart
