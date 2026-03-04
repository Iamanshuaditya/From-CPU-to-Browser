# Mental Models for Frontend Systems

This file gives simple, operational models that help you reason correctly under pressure.

## Why Browsers Are Complex
Browser = operating system for web apps.
It handles:
- process isolation and security
- networking and caching
- multiple runtimes (rendering + JS)
- scheduling across UI, I/O, and script work

### Practical model
Think of browser as a distributed runtime inside one app.

## Why the Rendering Pipeline Exists
Raw DOM is not directly drawable. Browser needs staged transforms:
- structure (DOM)
- style resolution
- geometry (layout)
- drawing instructions (paint)
- GPU composition

### Practical model
Rendering is a compiler pipeline from document description to pixels.

## Why JS Engines Are Built This Way
JS is dynamic; engines must balance correctness and speed.
- parse and interpret quickly for startup
- optimize hot paths later
- manage memory with GC due to dynamic object lifetimes

### Practical model
JS engine is a runtime optimizer with fallback safety paths.

## Why React Exists
Direct UI updates at scale are hard to reason about.
React provides:
- declarative UI model
- deterministic state-driven updates
- scheduling and batched commits

### Practical model
React is a change management layer for UI complexity.

## Why VDOM Exists
VDOM gives a stable representation to compute minimal DOM changes with predictable rules.

### Practical model
VDOM is a diffable intermediate representation for UI updates.

## Why Hydration and Streaming SSR Matter
- SSR improves first content display.
- Hydration adds interactivity after HTML arrives.
- Streaming SSR overlaps server render and network delivery.

### Practical model
SSR is for faster initial visibility; hydration is startup CPU debt you must budget.

## Common Misconceptions (and Fixes)

### Misconception 1
"Fast JS means fast app."

Fix:
Network, layout, and paint can dominate even when JS is optimized.

### Misconception 2
"Re-render equals DOM rewrite."

Fix:
Render phase can be pure computation; commit phase does host mutations.

### Misconception 3
"CSS is free."

Fix:
Selector complexity and broad invalidation can cause heavy style/layout work.

### Misconception 4
"SSR solves performance by default."

Fix:
Poor hydration strategy can shift cost to client CPU and hurt responsiveness.

### Misconception 5
"GC means memory is handled automatically."

Fix:
Unbounded references still cause leaks and pause spikes.

## Debugging Lens
When app is slow, ask in order:
1. Network bottleneck?
2. Main-thread long tasks?
3. Layout/paint invalidation problem?
4. Memory leak or GC churn?
5. Framework scheduling or update pattern issue?

## Related Guides
- [docs/frontend-systems-thinking.md](frontend-systems-thinking.md)
- [debugging/perf-debugging.md](../debugging/perf-debugging.md)
