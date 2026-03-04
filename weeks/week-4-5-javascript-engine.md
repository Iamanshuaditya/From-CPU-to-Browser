# Weeks 4-5 Textbook - JavaScript Engine

## How to Use This Chapter
- Target study time: 24-32 hours.
- Week 4: parser, AST, bytecode VM, closures/prototypes.
- Week 5: GC, event loop, async semantics, optimization.

## Learning Outcomes
You should be able to:
- Implement parsing pipeline and scope resolution.
- Execute bytecode with a stack VM.
- Implement closures, prototype property lookup.
- Build and reason about GC behavior.
- Model microtask/macrotask ordering correctly.

---

## Chapter 1 - Language Frontend Architecture

### 1.1 Pipeline
```text
source -> lexer -> parser -> AST -> scope analysis -> bytecode
```

### 1.2 Why Scope Analysis Is Separate
Name resolution and closure capture decisions should be explicit before execution.

---

## Chapter 2 - Lexer and Parser Internals

### 2.1 Lexer Rules
Token classes: identifiers, keywords, literals, punctuators/operators.

### 2.2 Parser Strategy
Recursive descent + precedence handling for expressions.

### 2.3 Error Reporting
Include token span and expected grammar element.

---

## Chapter 3 - AST and Symbol Resolution

### 3.1 AST Design
Each node stores kind, children, and source range.

### 3.2 Scope Tree
Nested lexical scopes track declarations and captures.

### 3.3 Closure Capture Marking
Mark variables escaping current frame so they move to heap-backed environments.

---

## Chapter 4 - Bytecode and VM Design

### 4.1 Stack VM Model
- operand stack for expression evaluation
- call frames for locals/instruction pointer

### 4.2 Instruction Set Example
- load/store local/global
- arithmetic/comparison
- jumps/calls/returns
- property access ops

### 4.3 Diagram: VM Execution
```text
[bytecode stream] + [ip] + [operand stack] + [call stack] + [heap]
```

---

## Chapter 5 - Objects, Prototypes, and Inline Caches

### 5.1 Object Storage
Map-like property storage plus prototype pointer.

### 5.2 Prototype Chain Lookup
`obj.prop` checks own properties then walks prototype chain.

### 5.3 Inline Cache Idea
Cache receiver shape and offset at hot property access sites.

### 5.4 Common Bug
Shape changes invalidate caches; stale cache entries produce incorrect reads unless guarded.

---

## Chapter 6 - Closures and Environments

### 6.1 Lexical Environment Model
Function captures environment pointer for free variables.

### 6.2 Escaping Variables
Variables referenced by inner functions must outlive caller frame.

### 6.3 Diagram: Closure Chain
```text
inner fn -> env2 -> env1 -> global env
```

---

## Chapter 7 - Garbage Collection

### 7.1 Mark-and-Sweep Basics
1. Collect roots
2. Traverse reachable objects
3. Sweep unreachable objects

### 7.2 Generational Model
- young generation for short-lived objects
- old generation for survivors
- promotion policy after surviving cycles

### 7.3 GC Metrics
Track pause time, reclaimed bytes, promotion rate, allocation rate.

### 7.4 Write Barrier Need
Old-to-young references require barriers so minor GC remains correct.

---

## Chapter 8 - Event Loop and Async Semantics

### 8.1 Queues
- macrotasks: timers, I/O callbacks
- microtasks: promise reactions

### 8.2 Ordering Rule
After each macrotask, drain microtasks before next rendering opportunity.

### 8.3 Async/Await Mapping
`await` schedules continuation as microtask after promise settles.

### 8.4 Diagram: Loop Tick
```text
macrotask -> drain microtasks -> render chance -> next macrotask
```

---

## Chapter 9 - Optional JIT Concepts

### 9.1 Hot Path Detection
Track call counts/type feedback.

### 9.2 Speculative Optimization
Assume stable types/shapes, generate fast path, keep bailout path.

### 9.3 Deoptimization
If assumptions break, revert to baseline execution preserving correctness.

---

## Quiz A
1. Why do closures force some variables onto heap-backed environments?
2. Why is scope resolution usually done before execution?
3. What does an inline cache optimize?
4. Why are write barriers required with generational GC?
5. Why can microtask-heavy code starve rendering?
6. What causes deoptimization in a speculative optimizer?
7. Why is VM stack discipline crucial for correctness?
8. What makes property lookup expensive without caching?

## Quiz A Answers
1. They must outlive the stack frame where declared.
2. It resolves bindings/captures deterministically and simplifies runtime lookups.
3. Repeated property access by caching shape/offset for fast-path reads.
4. Minor GC must still find young objects referenced from old objects.
5. Because microtasks are drained before render opportunity.
6. Runtime type/shape assumptions becoming false.
7. Corrupted stack state breaks calls, returns, and operand correctness.
8. Prototype chain traversal and dynamic property checks.

---

## Practice Problems (Solved)

### Problem 1: Closure Bug
Inner function reads stale value after outer function returns.

**Solution**
Captured variable likely stored only on stack. Move captured bindings to heap environment.

### Problem 2: Property Access Slowdown
Hot loop spends time in `GetProp`.

**Solution**
Add inline cache with shape guards and fallback slow path.

### Problem 3: Long GC Pauses
App freezes during allocation bursts.

**Solution**
Increase young-gen efficiency, reduce allocation churn, and evaluate incremental collection steps.

### Problem 4: Async Ordering Surprise
`Promise.then` runs before `setTimeout(0)` callback.

**Solution**
Correct behavior: microtasks run before next macrotask.

---

## Lab Pack

### Lab 1 - Parser + AST Snapshots
Parse representative programs; compare AST output against golden snapshots.

### Lab 2 - VM Opcode Tests
Unit-test each bytecode instruction and stack effect.

### Lab 3 - Closure Semantics Suite
Nested closures and mutation tests validating lexical capture behavior.

### Lab 4 - GC Stress Harness
Allocation-heavy benchmark with forced GC cycles and pause metrics.

### Lab 5 - Event Loop Ordering Tests
Systematically test promise/timer ordering edge cases.

## Completion Checklist
- Parser and scope resolver are deterministic.
- VM executes control flow and function calls correctly.
- GC reclaims unreachable objects safely.
- Async ordering matches JS semantics for tested cases.
