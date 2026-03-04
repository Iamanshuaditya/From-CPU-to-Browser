# Event Loop Labs

## Lab 1: Ordering Basics
Test and explain ordering for:
- sync code
- `setTimeout`
- Promise microtasks

Expected takeaway:
- microtasks drain before next macrotask.

## Lab 2: Nested Scheduling
Create nested `setTimeout` and nested Promise chains.

Expected takeaway:
- understand starvation risks and queue behavior.

## Lab 3: Render Opportunity Simulation
Insert a simulated render step between task ticks.

Expected takeaway:
- long microtask chains can delay rendering.

## Lab 4: Backpressure Scenario
Feed high-rate macrotasks and observe queue growth.

Expected takeaway:
- scheduling policy directly impacts responsiveness.
