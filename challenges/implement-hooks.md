# Challenge: Implement Hooks (`useState`, `useEffect`)

**Difficulty:** medium

## Objective
Implement core hooks in mini-react with lifecycle-correct behavior.

## Scope
- hook slot indexing
- update queue for `useState`
- dependency diff + cleanup for `useEffect`

## Acceptance Criteria
- [ ] Hook order invariant enforced.
- [ ] `useState` supports multiple updates in one tick.
- [ ] `useEffect` runs cleanup before re-run when deps change.
- [ ] Test suite covers stale closure and dependency edge cases.
- [ ] Render vs commit semantics documented.

## Submission Requirements
- implementation notes
- test matrix
- demo component showing correct lifecycle behavior
