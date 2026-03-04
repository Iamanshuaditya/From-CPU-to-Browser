# Case Study: React Rendering Cost

## Why Rerenders Happen
Rerenders can be triggered by:
- state updates
- parent rerenders propagating down
- context updates
- prop identity changes (new references)

Not all rerenders are bad, but unnecessary rerenders waste CPU and can increase commit time.

## Render Time vs Commit Time
- Render phase: compute next UI representation.
- Commit phase: apply DOM mutations and effects.

High render time suggests computation/scheduling issues.
High commit time suggests DOM mutation, layout, or paint pressure.

## Profiler Usage Guide
1. Open React DevTools Profiler.
2. Record a slow interaction.
3. Identify components with highest self/total render cost.
4. Check why component rendered (props/state/context).
5. Correlate with browser Performance panel for commit/layout/paint costs.

## Optimization Patterns
- Memoize components where prop stability exists.
- Stabilize callback/object references where it reduces rerenders.
- Split large components to isolate update scope.
- Virtualize long lists.
- Move heavy synchronous work off interaction path.

## Tradeoffs
- Over-memoization can add complexity and memory overhead.
- Stabilizing references without need can reduce readability.
- Aggressive splitting can increase architecture overhead.

## What to Measure
- render duration per interaction
- commit duration per interaction
- number of components rerendered
- long tasks and dropped frames

## Before/After Example
- Before: 120 components rerendered, 85ms render + 40ms commit.
- After: 35 components rerendered, 28ms render + 18ms commit.
- Net: lower interaction latency and smoother UI.
