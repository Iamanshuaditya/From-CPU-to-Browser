# Mini React Requirements

## Functional Requirements
- Define VDOM node format.
- Render initial VDOM tree to host DOM.
- Diff old/new trees and generate patch set.
- Support keyed list reconciliation.
- Batch updates into commit phase.
- Implement `useState` and `useEffect` basics.

## Correctness Requirements
- Stable hook order invariants enforced.
- `useEffect` dependency and cleanup behavior tested.
- keyed reorder behavior preserves logical identity.

## Instrumentation Requirements
- measure render phase time
- measure commit phase time
- count host DOM operations per update

## Stretch Goals
- lane-like priority scheduling
- transition-style non-urgent updates
