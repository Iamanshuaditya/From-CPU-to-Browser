# HTML+CSS Renderer Requirements

## Functional Requirements
- Parse basic HTML into a tree.
- Parse CSS rules with simple selectors:
  - type
  - class
  - id
  - descendant combinator
- Compute specificity and cascade winner.
- Compute inherited and explicit style values for supported properties.
- Build layout tree and compute box geometry for block and inline flow subset.

## Required Supported Properties (minimum)
- `display`
- `width`/`height`
- `margin`/`padding`/`border`
- `font-size`
- `color`
- `background-color`

## Debuggability Requirements
- Print DOM tree.
- Print matched selectors per node.
- Print computed style per node.
- Print layout boxes with x/y/width/height.

## Correctness Requirements
- Malformed HTML and CSS should not crash.
- Invalid CSS declarations should be ignored safely.
- Layout output must be deterministic for fixed inputs.

## Stretch Goals
- flexbox subset
- media queries subset
- style invalidation on DOM mutation
