# Mini Browser Performance Checkpoints

Measure these checkpoints at each milestone.

## Checkpoint A: Baseline
- parse/style/layout/paint/composite timing
- total frame time

## Checkpoint B: Complex DOM
- run with large DOM tree
- compare stage percentages

## Checkpoint C: Style Stress
- many CSS rules and selector depth
- measure style computation impact

## Checkpoint D: Layout Stress
- deep nesting + mixed layout modes
- measure layout scaling behavior

## Checkpoint E: Optimization Pass
- apply one targeted optimization
- show before/after with same dataset

## Reporting Format
- environment
- input size/profile
- metrics table
- interpretation
- next bottleneck hypothesis
