# Weeks 2-3 Textbook - Browser Engine Implementation

## How to Use This Chapter
- Target study time: 24-32 hours.
- Week 2 focuses on parsing + style.
- Week 3 focuses on layout + paint + compositing.
- Build while reading; do not postpone implementation.

## Learning Outcomes
You should be able to:
- Convert HTML/CSS input into styled layout trees.
- Compute box geometry for block/inline/flex/grid subsets.
- Generate display lists and composite layers.
- Instrument each render pipeline stage.

---

## Chapter 1 - Rendering Pipeline Overview

### 1.1 Pipeline Stages
```text
HTML bytes -> HTML tokenizer -> DOM tree
CSS bytes  -> CSS tokenizer  -> CSSOM
DOM + CSSOM -> Style computation -> Layout tree -> Paint list -> Composite
```

### 1.2 Invalidation Model
Any DOM/style change can trigger:
- style recalculation
- layout
- paint
- composite

Avoid broad invalidation; scope it.

---

## Chapter 2 - HTML Tokenizer Deep Dive

### 2.1 Token Types
- start tag
- end tag
- text
- comment
- doctype

### 2.2 State Machine Design
Tokenizer should be deterministic and incremental.

### 2.3 Error Recovery Principle
Malformed HTML should not crash parser. It should emit recoverable tokens and continue.

### 2.4 Diagram: Tokenizer Loop
```text
read char -> transition(state,char) -> maybe emit token -> next char
```

---

## Chapter 3 - Tree Construction

### 3.1 Stack of Open Elements
Maintains parent-child context while parsing.

### 3.2 Insertion Modes
Use simplified modes to maintain correctness in head/body transitions.

### 3.3 Implied End Tags
Parser should auto-close certain structures when malformed nesting occurs.

### 3.4 Practical Pitfall
Naive tree builder fails on missing closing tags; robust builders recover gracefully.

---

## Chapter 4 - Script Interaction with Parser

### 4.1 Blocking Scripts
Classic scripts can pause parser and mutate DOM synchronously.

### 4.2 `async` vs `defer`
- `async`: execute when ready, order not guaranteed.
- `defer`: execute after parsing, in source order.

### 4.3 Why It Matters
Scheduling semantics affect DOM readiness and rendering order.

---

## Chapter 5 - CSS Parsing and Cascade

### 5.1 CSS Tokenization/Parsing
Implement grammar for selectors + declaration blocks.

### 5.2 Selector Matching
Start with:
- type, class, id
- descendant combinator

### 5.3 Specificity
Use tuple `(id, class/attr/pseudo, type)`.

### 5.4 Cascade Resolution
Tie-break by:
1. importance/origin (simplified)
2. specificity
3. source order

### 5.5 Diagram: Style Resolution
```text
Matched rules -> sort by cascade -> final property map
```

---

## Chapter 6 - Computed Style and Inheritance

### 6.1 Stages
- specified value
- computed value
- used value (after layout context)

### 6.2 Inherited Properties
Typography-like properties often inherit; geometry properties generally do not.

### 6.3 Media Queries
Evaluate query conditions before activating associated rules.

---

## Chapter 7 - Layout Engine Foundations

### 7.1 Layout Tree vs DOM Tree
Layout tree excludes non-rendered nodes and may add anonymous boxes.

### 7.2 Box Model
Content + padding + border + margin determines final geometry.

### 7.3 Block/Inline Formatting
- block flow computes widths/heights in flow context.
- inline flow builds line boxes and wraps text.

### 7.4 Diagram: Box Model
```text
+---------------- margin ----------------+
| +-------------- border --------------+ |
| | +----------- padding ------------+ | |
| | |            content            | | |
| | +-------------------------------+ | |
| +-----------------------------------+ |
+---------------------------------------+
```

---

## Chapter 8 - Flexbox and Grid Core Algorithms

### 8.1 Flexbox Subset
1. Determine flex basis.
2. Build flex lines.
3. Resolve free space.
4. Grow/shrink per factors.
5. Align on cross axis.

### 8.2 Grid Subset
- explicit tracks
- auto-placement
- spanning and track sizing

### 8.3 Debug Rule
Log intermediate values for each phase. Layout bugs are often arithmetic or phase-order bugs.

---

## Chapter 9 - Paint System

### 9.1 Display List Concept
Paint list decouples drawing decisions from raster backend.

### 9.2 Paint Order
Respect stacking and z-order assumptions to avoid visual glitches.

### 9.3 Command Types
- draw rect
- draw border
- draw text
- draw image

---

## Chapter 10 - Layers and Compositing

### 10.1 Why Layers Exist
Transform/opacity/scrolling often benefit from separate composited layers.

### 10.2 Pipeline
```text
Display list -> layerization -> raster tiles/bitmaps -> compositor merge
```

### 10.3 Cost Tradeoff
More layers can reduce repaint cost but increase memory/compositor overhead.

---

## Chapter 11 - Instrumentation and Metrics

Collect for each render pass:
- parse time
- style time
- layout time
- paint list build time
- raster/composite time
- total frame time

Use percentile metrics, not just average.

---

## Quiz A
1. Why separate DOM and layout trees?
2. What role does specificity play in cascade?
3. Why can script loading mode affect render correctness?
4. What is the benefit of display lists?
5. Why can too many layers hurt performance?
6. Which stage usually dominates in complex UI reflows?
7. Why are inline formatting contexts tricky?
8. What is the relationship between invalidation scope and frame time?

## Quiz A Answers
1. Layout needs render-centric structure and anonymous box handling not represented directly in DOM.
2. It ranks conflicting selectors before source-order tie-break.
3. Blocking/async/defer change DOM mutation timing and readiness ordering.
4. It isolates paint decisions from backend rasterization and aids debugging/caching.
5. Layer memory and compositing costs increase.
6. Often layout, especially with broad invalidation.
7. Line breaking, font metrics, and inline box interactions are complex.
8. Broader invalidation increases recomputation and frame cost.

---

## Practice Problems (Solved)

### Problem 1: Selector Conflict
`#id .title` and `.card .title` both set color. Which wins and why?

**Solution**
`#id .title` has higher specificity due to id selector.

### Problem 2: Layout Bug
A child overflows unexpectedly after adding border/padding.

**Solution**
Re-check box model calculations and width constraints (content vs border-box assumptions).

### Problem 3: Jank on Resize
Window resize triggers full-page layout each frame.

**Solution**
Reduce invalidation scope, throttle resize handler, and avoid sync layout thrash in JS.

### Problem 4: Paint Spike
Small DOM update triggers huge paint cost.

**Solution**
Likely large repaint region or layer invalidation. Inspect invalidation rectangles and layer boundaries.

---

## Lab Pack

### Lab 1 - HTML Parser Robustness
Feed malformed HTML corpus; ensure no crashes and reasonable tree recovery.

### Lab 2 - Specificity Test Suite
Create 30 cascade conflict tests and assert computed results.

### Lab 3 - Layout Snapshot Tests
Export geometry JSON and compare against golden files.

### Lab 4 - Paint/Composite Profiling
Benchmark simple vs complex pages and compare stage timing percentages.

### Lab 5 - Wikipedia Subset Render
Render real pages with text/images/basic styles and collect metrics report.

## Completion Checklist
- Parser handles malformed input safely.
- Style and specificity results are test-covered.
- Layout supports block/inline/flex/grid subsets.
- Metrics clearly identify pipeline bottlenecks.
