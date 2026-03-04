# Tests and Invariants

## Invariants
- Hook order must remain consistent between renders.
- Commit phase is the only place for host side effects.
- Keyed children preserve identity across reorders.
- Effects with unchanged deps do not rerun.

## Required Tests
- initial render correctness
- nested update correctness
- keyed list reorder behavior
- effect cleanup ordering
- batched state update behavior
- render vs commit timing capture

## Suggested Test Matrix
- small tree / large tree
- frequent updates / sparse updates
- stable keys / unstable keys
- shallow effects / deep effect chains
