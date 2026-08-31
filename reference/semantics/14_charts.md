# 14 - Charts

Status: STRONGLY GROUNDED

Charts are separate from low-level canvas graphics.

Current formal action family:

```text
CHART_BEGIN
CHART_VALUES_FROM_FILE
CHART_VALUES_FROM_ARRAY
CHART_VALUES
CHART_FROM_FILE
CHART_BIND
CHART_ARRAY_VALUES
CHART_ARRAY_LABELS
CHART_LABELS_FROM_ARRAY
CHART_ARRAY_COLORS
CHART_COLORS_FROM_ARRAY
CHART_SET
CHART_LINE
CHART_DRAW
```

## Lifecycle

Canonical order:

```baselscript
CHART_BEGIN type=<type> [bg=<color>]
CHART_SET ...
<data source>
[CHART_BIND ...]
CHART_DRAW
```

`CHART_BEGIN` resets chart state.

## Confirmed chart types

Observed current types include:

```text
bars
grouped
stack
line
scatter
lollipop
dot
pie
donut
```

Runtime aliases include:

```text
bar -> bars
stacked -> stack
```

Do not invent chart types.

## Array-driven example

```baselscript
CHART_BEGIN type=pie bg=#efefef
CHART_SET title="Pie Diagram"
CHART_SET subtitle="Basic Proportions"

CHART_VALUES_FROM_ARRAY #values_array=(100,160,110,180,120,50)
CHART_LABELS_FROM_ARRAY #labels_array=("A","B","C","D","E","F")

CHART_DRAW
```

## File-driven example

```baselscript
CHART_VALUES_FROM_FILE file=#file_name \
    record=(#date:"DATE (dd.mm)",#value:"VALUE (kg)") header=yes
```

Observed parameters include:

```text
file
id
dir
record
header
```

## CHART_BIND

Examples:

```baselscript
CHART_BIND x=#date y=(#columnA:Apples,#columnB:Bananas)
CHART_BIND values=(#values_array) labels=#labels_array colors=#colors_array
```

## CHART_SET

Repeated keys include title/subtitle, axis titles, colors, legend, grid, and type. Use only keys backed by current reference/examples.


## Weak parameter contracts

`CHART_VALUES` and `CHART_LINE` are CURRENT action names, but the machine contract does not currently export a complete argument grammar for them.

Therefore:

```text
action exists != parameter layout is known
```

Do not generate a new `CHART_VALUES ...` or `CHART_LINE ...` parameter form unless an exact current reference/example for that form has been loaded.

The same rule applies to any CHART action whose routed reference does not provide the required arguments.

## Prefix rule

Specific action names must precede overlapping general prefixes:

```text
CHART_VALUES_FROM_FILE
CHART_VALUES_FROM_ARRAY
CHART_VALUES
```

## Generation rules

- prefer high-level CHART actions for chart requests;
- do not rebuild a chart from low-level `draw` unless explicitly requested;
- do not invent CHART_* actions or argument layouts by naming analogy.
