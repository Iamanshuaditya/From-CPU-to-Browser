# Mini Browser Requirements

## Functional Requirements
- Parse HTML and build DOM.
- Parse CSS and compute style values.
- Build layout tree and compute geometry.
- Generate paint commands.
- Output frame or debug representation of final render.

## Instrumentation Requirements
Record timings for:
- parse
- style
- layout
- paint
- composite

## Correctness Requirements
- Graceful handling of malformed HTML/CSS.
- Deterministic output for same input.
- Snapshot tests for representative documents.

## Stretch Goals
- image rendering support
- basic animations
- incremental reflow/paint invalidation
