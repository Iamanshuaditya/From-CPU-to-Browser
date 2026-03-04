# Experiment: DOM Size Impact

## Goal
Understand how DOM size affects parse, style, layout, and interaction performance.

## Setup
- Generate pages with 1k, 5k, 10k, and 50k nodes.
- Run same interaction/update sequence on each.

## What to Measure
- first render time
- style recalculation time
- layout and paint time
- input latency during updates

## Expected Outcome
Larger DOM sizes increase baseline render cost and amplify update-time bottlenecks.

## Why It Matters
DOM growth often happens gradually in real apps; this experiment helps define safe operating budgets.
