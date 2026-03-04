# Mini JS Runtime Requirements

## Functional Requirements
- Parse a JS subset (variables, functions, calls, conditionals, loops).
- Execute code with stack frames and scope handling.
- Support closure capture behavior.
- Support object properties and prototype lookup baseline.
- Implement microtask and macrotask queue semantics.
- Implement mark-and-sweep garbage collection.

## Correctness Requirements
- Deterministic event loop ordering tests.
- Closure semantics validated with test cases.
- No use-after-free behavior in GC flows.

## Observability Requirements
- opcode/execution counters (if bytecode VM)
- queue length and event loop tick metrics
- GC pause duration and reclaimed objects count

## Stretch Goals
- generational GC
- inline cache for property lookup
- async/await lowering
