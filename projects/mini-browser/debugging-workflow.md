# Mini Browser Debugging Workflow

## Stage-by-Stage Debugging Strategy
1. Freeze one failing input document.
2. Dump each stage output.
3. Find first stage where output diverges from expected.
4. Fix and add regression test.

## Common Failure Classes
- Parse tree shape mismatch.
- Incorrect selector match and cascade winner.
- Wrong layout width/height due to box-model errors.
- Paint order bugs (z-order/stacking confusion).

## Tooling
- Snapshot files for DOM/style/layout/paint.
- Time each stage with scoped timers.
- Use differential comparison between revisions.

## Quick Triage Tree

```mermaid
flowchart TD
    A[Visual Bug] --> B{DOM correct?}
    B -- No --> C[Fix parser/tree builder]
    B -- Yes --> D{Styles correct?}
    D -- No --> E[Fix selector/cascade]
    D -- Yes --> F{Geometry correct?}
    F -- No --> G[Fix layout calculations]
    F -- Yes --> H[Fix paint/composite order]
```
