# Experiment: Event Loop Order

## Goal
Validate ordering semantics of sync code, macrotasks, and microtasks.

## Setup
Create a script with:
- synchronous logs
- `setTimeout`
- Promise chains
- nested scheduling

## What to Measure
- exact execution order
- queue growth under stress
- delay between task scheduling and execution

## Expected Outcome
Microtasks run after current call stack and before next macrotask.

## Why It Matters
Incorrect event loop assumptions cause UI glitches, race conditions, and startup stalls.
