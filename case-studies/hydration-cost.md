# Case Study: Hydration Cost

## What Hydration Does
Hydration attaches event listeners and runtime state to server-rendered HTML so the page becomes interactive.

## Why It Is Expensive
- JS parse + execute cost on startup.
- Component tree traversal and binding work.
- Main-thread competition with rendering and user input.
- Potential re-render mismatches causing extra work.

## How Streaming SSR Changes the Game
Streaming SSR sends HTML chunks progressively:
- improves first visible content timing
- lets browser begin parsing earlier
- can pair with selective hydration to prioritize critical UI areas

## What to Measure
- Time to first content (FCP/LCP context).
- Time to interactive response after initial load.
- Hydration duration on main thread.
- Long tasks during hydration window.
- Mismatch warnings and fallback rerender costs.

## Optimization Playbook
1. Split bundles and defer non-critical code.
2. Prioritize hydration for above-the-fold interactive regions.
3. Reduce hydration scope for static sections.
4. Remove non-deterministic server/client render differences.
5. Validate third-party script impact during hydration window.

## Example Experiment Plan
- Baseline: full hydration without prioritization.
- Variant A: selective hydration + route-level splitting.
- Variant B: streaming SSR + selective hydration.
- Compare p50/p95 startup responsiveness.

## How to Fix Common Issues
- Mismatch warnings: ensure deterministic render inputs.
- Long tasks: split work and defer low-priority components.
- User input delay: prioritize interaction-critical hydration boundaries.
