# Case Study: Layout Thrashing

## What It Is
Layout thrashing happens when code repeatedly forces synchronous layout by mixing DOM writes and layout reads in tight loops.

## Minimal Reproduction (Pseudocode)
```js
for (const el of elements) {
  el.style.width = computeNextWidth(el);   // write
  const h = el.getBoundingClientRect().height; // read forces layout
  el.style.height = h + 10 + 'px';         // write
}
```

Why this hurts:
- Browser repeatedly recalculates layout between operations.
- Main thread gets blocked by forced reflow work.

## How to Detect in DevTools
1. Open Performance panel.
2. Record interaction causing jank.
3. Look for repeated "Layout" tasks interleaved with scripting.
4. Inspect call stack for sync layout reads (`getBoundingClientRect`, `offsetHeight`, computed style reads).

## How to Fix
- Batch reads first, then writes.
- Cache measurements when possible.
- Reduce number of invalidated elements.
- Use transform/opacity for visual updates that do not require layout.

Refactored pattern:
```js
const heights = elements.map(el => el.getBoundingClientRect().height); // read batch
for (let i = 0; i < elements.length; i++) {
  elements[i].style.height = heights[i] + 10 + 'px'; // write batch
}
```

## Metrics to Track
- Total layout time per interaction.
- Number of forced layout events.
- Main-thread long task count.
- Interaction latency (INP/FID proxy).

## Before/After Template
- Baseline: layout time, long tasks, input delay.
- Change: read/write batching and invalidation narrowing.
- Result: measured deltas with same profile.

## Common Tradeoffs
- More temporary arrays/caching can increase memory.
- Batching may complicate code structure.
