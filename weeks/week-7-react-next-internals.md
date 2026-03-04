# Week 7 Textbook - React and Next.js Internals

## How to Use This Chapter
- Target study time: 14-18 hours.
- Build a minimal React-like runtime while reading.
- Validate each concept with runnable examples.

## Learning Outcomes
- Explain Fiber architecture and interruptible rendering.
- Implement core hook mechanics.
- Reason about concurrent features and transitions.
- Understand SSR streaming and hydration tradeoffs.
- Map Next.js routing and rendering strategies.

---

## Chapter 1 - Fiber Model

### 1.1 Motivation
Recursive reconciliation is hard to pause. Fiber stores units of work explicitly to support scheduling.

### 1.2 Fiber Node Essentials
- type/key/props
- child/sibling/return links
- alternate pointer
- effect flags

### 1.3 Diagram
```text
current fiber tree <-> work-in-progress (alternate) tree
```

---

## Chapter 2 - Render vs Commit Phases

### 2.1 Render Phase
Pure computation, interruptible, restartable.

### 2.2 Commit Phase
Applies side effects and host mutations; must be consistent and non-interruptible.

### 2.3 Practical Rule
Never perform irreversible side effects during render.

---

## Chapter 3 - Lanes and Priority

### 3.1 Lane Concept
Lanes represent urgency classes for pending updates.

### 3.2 Scheduling Outcome
Urgent updates can preempt less urgent work, improving input responsiveness.

---

## Chapter 4 - Hook Internals

### 4.1 Hook Order Constraint
Hooks are indexed by call order; conditional hook calls break state mapping.

### 4.2 `useState`
Queue updates per hook; compute next state during render.

### 4.3 `useEffect`
Collect effects during render; run cleanup and setup in commit/post-commit phases.

### 4.4 Diagram
```text
render -> build effect list -> commit DOM -> run passive effects
```

---

## Chapter 5 - Concurrent Features

### 5.1 Time Slicing
Process work in chunks and yield.

### 5.2 Suspense
Render fallback while async boundary resolves.

### 5.3 Transitions
Mark non-urgent updates to avoid input stalls.

---

## Chapter 6 - SSR and Hydration

### 6.1 `renderToString` vs Streaming
- string render: full blocking output
- streaming: progressive HTML chunks, better TTFB/perceived speed

### 6.2 Hydration
Attach behavior to server-rendered HTML without full re-render.

### 6.3 Hydration Mismatch
Occurs when server and client renders differ; needs diagnostics and deterministic rendering practices.

---

## Chapter 7 - Next.js Architectural Concepts

### 7.1 File-Based Routing
Route tree generated from filesystem conventions.

### 7.2 SSG/ISR
- SSG: build-time HTML
- ISR: timed revalidation and partial freshness model

### 7.3 Middleware and Edge
Request-time logic with runtime constraints and latency benefits.

---

## Quiz A
1. Why does Fiber use `alternate` trees?
2. Why must commit be non-interruptible?
3. What breaks when hook call order changes?
4. Why can streaming SSR improve perceived performance?
5. What is a transition update used for?
6. Why do hydration mismatches happen?
7. Which phase should schedule passive effects?
8. Why can lane priority reduce input lag?

## Quiz A Answers
1. Double buffering between current and work-in-progress trees.
2. Partial side-effect application would leave inconsistent UI.
3. Hook state slots map to wrong hooks.
4. User sees content earlier before full page computation completes.
5. Non-urgent updates that should not block urgent interaction.
6. Non-deterministic render output between server and client.
7. After commit (post-render side effects).
8. Urgent updates preempt slower background work.

---

## Practice Problems (Solved)

### Problem 1: Conditional Hook Crash
A component calls `useState` inside `if` and state becomes corrupted.

**Solution**
Move hook calls to top-level consistent order.

### Problem 2: Janky Input During Heavy Render
Typing lags while a large list updates.

**Solution**
Use priorities/time slicing and transition non-urgent updates.

### Problem 3: Hydration Warning
Server markup differs on first client render.

**Solution**
Remove non-deterministic values during SSR or serialize stable data from server.

---

## Lab Pack

### Lab 1 - Mini Fiber Loop
Implement begin/complete work traversal with cooperative yielding.

### Lab 2 - Hooks Runtime
Implement `useState` + `useEffect` with cleanup/dependency logic.

### Lab 3 - Streaming SSR Demo
Emit chunked HTML and hydrate interactively.

### Lab 4 - Mismatch Debug Tool
Add diagnostics that identify first mismatch node.

## Completion Checklist
- Fiber render/commit model is implemented and explainable.
- Hooks behavior matches expected lifecycle semantics.
- SSR and hydration flow works on representative examples.
