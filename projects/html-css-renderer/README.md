# Project: HTML+CSS Parser + Tiny Layout Engine

Build a minimal browser core that can parse HTML/CSS and compute layout geometry.

## Why This Project
It connects parsing theory to visible rendering behavior and makes cascade/layout bugs concrete.

## Build Scope
- HTML tokenizer and tree builder
- CSS tokenizer/parser
- selector matching + specificity
- computed style generation
- tiny layout engine (block + inline baseline)

## Required Outputs
- parser tests
- style resolution tests
- layout snapshot tests
- debug dump for DOM/style/layout trees

## Documents
- [requirements.md](requirements.md)
- [milestones.md](milestones.md)
- [rendering-pipeline-mapping.md](rendering-pipeline-mapping.md)
- [rubric.md](rubric.md)

## Suggested Implementation Order
1. HTML tokenizer + DOM construction.
2. CSS parser + rule model.
3. Selector matching + cascade.
4. Computed style layer.
5. Layout tree + geometry passes.
6. Snapshot validation.
