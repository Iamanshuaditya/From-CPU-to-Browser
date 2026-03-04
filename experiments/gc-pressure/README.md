# Experiment: GC Pressure

## Goal
Measure runtime behavior under high allocation churn and compare optimization strategies.

## Setup
- Implement workload with frequent object allocation/deallocation.
- Add variant with object reuse/pooling.
- Keep functional behavior equivalent.

## What to Measure
- GC pause frequency and duration
- memory footprint over time
- throughput/latency under load

## Expected Outcome
Object reuse and reduced churn should lower GC pressure and improve responsiveness.

## Why It Matters
GC pressure is a common source of interaction stutter and p95 latency spikes in web apps.
