# Experiment: HTTP/1.1 vs HTTP/2

## Goal
Compare page load behavior across protocols under similar payload and network conditions.

## Setup
- Serve identical assets over HTTP/1.1 and HTTP/2.
- Use same host, compression, and cache profile.
- Run under normal and constrained network profiles.

## What to Measure
- total load time
- TTFB and connection timing
- request waterfall behavior
- resource contention patterns

## Expected Outcome
HTTP/2 typically improves multiplexing efficiency and request scheduling for asset-heavy pages.

## Why It Matters
Protocol-level behavior influences frontend performance without any app code changes.
