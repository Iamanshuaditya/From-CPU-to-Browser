# Hooks Implementation Notes

## Core Rule
Hooks are indexed by call order, not name.

## `useState` Model
- each hook stores current state and queue of pending updates
- render phase computes new state by replaying updates

## `useEffect` Model
- record effect + deps during render
- compare deps with previous render
- on commit:
  - run cleanup for changed effects
  - schedule/run current effects

## Common Bug Patterns
- conditional hooks changing order
- stale closure values due to missing dependencies
- effect cleanup not running on dependency change

## Minimal Internal Data Shape
- hook cursor per component/fiber
- linked list or array of hook slots
- pending effect list for commit phase
