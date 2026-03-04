# Experiment: Layout Cost

## Goal
Measure how layout complexity scales with DOM depth, style complexity, and update patterns.

## Setup
- Build pages with increasing DOM depth.
- Apply style changes at different subtree levels.
- Compare single large invalidation vs targeted invalidation.

## What to Measure
- layout duration per update
- number of invalidated nodes
- frame time under repeated updates

## Expected Outcome
Broad invalidation and deep trees increase layout cost disproportionately.

## Why It Matters
Layout dominates many frontend bottlenecks, especially in dynamic dashboards and editors.
