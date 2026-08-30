# BaselScript Charts Status

## Purpose

This file documents the CURRENT BaselScript high-level chart subsystem for AI generation,
review, validation support, and maintenance.

Charts are a separate subsystem from low-level `draw tile=...` graphics. A chart is built
as state: begin/reset the chart, set presentation options, provide data, optionally bind
arrays to axes/series, and render it.

Do not replace the high-level CHART API with hand-built canvas graphics unless the user
explicitly asks for a low-level implementation.

## Authority and evidence

CURRENT chart actions formally present in the current action definitions and/or runtime:

- `CHART_BEGIN`
- `CHART_VALUES_FROM_FILE`
- `CHART_FROM_FILE`
- `CHART_BIND`
- `CHART_VALUES_FROM_ARRAY`
- `CHART_ARRAY_VALUES`
- `CHART_ARRAY_LABELS`
- `CHART_LABELS_FROM_ARRAY`
- `CHART_ARRAY_COLORS`
- `CHART_COLORS_FROM_ARRAY`
- `CHART_SET`
- `CHART_LINE`
- `CHART_DRAW`

Maintainer-confirmed CURRENT action:

- `CHART_VALUES`

`CHART_VALUES` was confirmed as working in the current interpreter by the BaselScript
maintainer during the 2026-08-30 training audit. It was missing from the older exported
`actions.def` and V3 corpus scan. Therefore this update registers the action name, but AI
must not invent a detailed argument contract for `CHART_VALUES` until a canonical runtime
example or machine-readable syntax is exported.

## Chart lifecycle

Canonical high-level order:

```baselscript
CHART_BEGIN type=<type> [bg=<color>]
CHART_SET ...
CHART_SET ...
<data source command>
[CHART_BIND ...]
CHART_DRAW
```

`CHART_BEGIN` is the reset boundary for chart state. Settings from one chart must not be
assumed to leak into the next chart.

## CHART_BEGIN

Repeated runtime and corpus usage supports:

```baselscript
CHART_BEGIN type=<type> bg=<color>
```

The runtime also accepts an optional orientation argument in existing implementations:

```baselscript
CHART_BEGIN type=pie bg=white orient=landscape
```

`orient=` is treated as optional/legacy in the current runtime comments. Prefer normal
scene/orientation logic unless an existing script already relies on it.

Runtime aliases:

```text
bar     -> bars
stacked -> stack
```

Observed CURRENT chart types in the real corpus:

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

Do not invent additional chart type names.

## Minimal array-driven chart

Canonical real corpus pattern for pie/donut charts:

```baselscript
SCENE=2 title="Pie Chart" mode=graphics
SECTION init

    CHART_BEGIN type=pie bg=#efefef

    CHART_SET title="Pie Diagram"
    CHART_SET subtitle="Basic Proportions"

    CHART_VALUES_FROM_ARRAY #values_array=(100,160,110,180,120,50)
    CHART_LABELS_FROM_ARRAY #labels_array=("Value N1","Value N2","Value N3","Value N4","Value N5","Value N6")

    CHART_DRAW
END
END SCENE 2
```

This pattern is preferred when the values and labels already exist as BaselScript arrays.

## Array data actions

Observed and runtime-supported families:

```text
CHART_VALUES_FROM_ARRAY
CHART_ARRAY_VALUES
CHART_ARRAY_LABELS
CHART_LABELS_FROM_ARRAY
CHART_ARRAY_COLORS
CHART_COLORS_FROM_ARRAY
```

The corpus repeatedly uses forms such as:

```baselscript
CHART_VALUES_FROM_ARRAY #values_array=(100,160,110,180,120,50)
CHART_LABELS_FROM_ARRAY #labels_array=("A","B","C","D","E","F")
```

`CHART_ARRAY_LABELS` and `CHART_LABELS_FROM_ARRAY` are runtime aliases of the same label
loading family. `CHART_ARRAY_COLORS` and `CHART_COLORS_FROM_ARRAY` are the corresponding
color-loading family.

Do not invent additional `CHART_*_FROM_ARRAY` names merely because the naming pattern looks
regular.

## File data actions

`CHART_VALUES_FROM_FILE` and `CHART_FROM_FILE` route to the same runtime family.

Canonical repeated corpus form:

```baselscript
CHART_VALUES_FROM_FILE file=APPDIR/files/examples/example_chart3 \
    record=(#date:"DATE (dd.mm)",#columnA:Apples,#columnB:Bananas,#columnC:Oranges) header=yes
```

A second real form uses `id=` and `dir=`:

```baselscript
CHART_VALUES_FROM_FILE id=example_chart3 \
    record=(#date,#Product_A,#Product_B,#Product_C) \
    dir=APPDIR/files/examples header=yes
```

Observed parameters include:

```text
file
id
dir
record
header
```

Preserve an existing working file model. Do not silently convert between path-based
`file=` and `id=`/`dir=` forms without checking the surrounding file model.

## CHART_BIND

`CHART_BIND` explicitly binds category and value arrays.

Repeated corpus examples:

```baselscript
CHART_BIND x=#date y=(#columnA:Apples,#columnB:Bananas,#columnC:Oranges)
```

For horizontal orientation the binding can be reversed:

```baselscript
CHART_BIND x=(#columnA:Apples,#columnB:Bananas,#columnC:Oranges) y=#date
```

Pie/donut examples also show:

```baselscript
CHART_BIND values=(#values_array) labels=#labels_array colors=#colors_array
```

Observed binding keys:

```text
x
y
values
labels
colors
```

Do not assume every chart type requires explicit `CHART_BIND`. Some file and array helpers
establish enough state for `CHART_DRAW` directly.

## CHART_SET

`CHART_SET` updates the current chart state. Multiple settings may be placed in one command
or split across multiple commands.

Examples:

```baselscript
CHART_SET title="Stacked Bar Diagram" subtitle="Enhanced Styling"
CHART_SET category_axis=x
CHART_SET x_title="DATE (dd.mm)"
CHART_SET y_title="VALUE (kg)"
CHART_SET colors=(#E07A5F,#F2CC0F,#81B29A)
CHART_SET legend=1
CHART_SET grid=10
```

The runtime explicitly supports `type=` in `CHART_SET` as well, including aliases
`bar -> bars` and `stacked -> stack`.

### Frequently confirmed CHART_SET keys

The V3 corpus repeatedly uses these keys:

```text
title
subtitle
x_title
y_title
category_axis
values_draw
values_font
values_color
style_values
style_title
style_subtitle
header_color
style_legend
style_legend_title
style_legend_items
legend
legend_cols
legend_gap
legend_percent
percent_digits
colors
color
grid
x_cat_step
tick_font
cat_font
axis_title_font
style_axes
style_axis_x
style_axis_y
style_axis_values
axis_color
axis_title_color
cat_color
tick_text_color
tick_color
grid_color
marker_style
marker_radius
dot_radius
markers
line_width
radius
donut_hole
donut_color
points
slot_width
title_font
subtitle_font
title_color
value_format
```

Runtime source additionally shows support for chart-state parameters such as:

```text
bg
type
axis_titles
series_fallback_color
legend_format
bar_span
```

This list is descriptive, not permission to create arbitrary new setting names. Generate
only keys confirmed by runtime/reference or repeated real examples.

## Orientation and category axis

Current chart scripts commonly switch category orientation explicitly:

```baselscript
if #_orientation==portrait
    CHART_SET category_axis=x
    CHART_SET x_title="DATE (dd.mm)"
    CHART_SET y_title="VALUE (kg)"
else
    CHART_SET category_axis=y
    CHART_SET x_title="VALUE (kg)"
    CHART_SET y_title="DATE (dd.mm)"
endif
```

`category_axis` accepts `x` or `y` in the current runtime.

## Pie and donut settings

Confirmed settings include:

```text
radius
donut_hole
donut_color
legend_percent
percent_digits
legend_cols
legend_gap
```

Example:

```baselscript
CHART_BEGIN type=donut bg=#efefef
CHART_SET radius=0.78
CHART_SET donut_hole=0.52
CHART_SET donut_color="#ffffff"
CHART_SET legend_percent=1
CHART_SET percent_digits=0
```

## Point, dot, scatter and line settings

Repeated corpus settings include:

```text
marker_radius
marker_style
dot_radius
markers
points
line_width
values_draw
```

Do not transfer a chart-specific parameter to unrelated chart types unless current runtime
or a working example confirms it.

## CHART_DRAW

`CHART_DRAW` is the render/finalization step.

Canonical form:

```baselscript
CHART_DRAW
```

Real donut examples also pass a render-time option:

```baselscript
CHART_DRAW legend_percent=1
```

Observed render-time parameters in the V3 corpus include:

```text
legend_percent
colors
```

Prefer `CHART_SET` for persistent chart configuration and use `CHART_DRAW` parameters only
when an existing supported pattern requires them.

## CHART_LINE

`CHART_LINE` is present in the current formal action definition and Windows runtime routing.
The V3 corpus has no normal leading-command example for it.

Status:

```text
CURRENT action name
runtime-confirmed
weak example coverage
```

AI should preserve existing `CHART_LINE` use but should not invent its detailed parameter
syntax without a canonical example or exported syntax contract.

## CHART_VALUES

`CHART_VALUES` is maintainer-confirmed CURRENT but was not represented in the older V3
corpus and exported action definition used at the start of this audit.

Status:

```text
CURRENT by maintainer/runtime confirmation
formal definition added in this training update
canonical argument syntax still requires export/example evidence
```

Until that evidence is present, AI may recognize and preserve `CHART_VALUES`, but must not
fabricate its arguments from the syntax of `CHART_VALUES_FROM_ARRAY` or
`CHART_VALUES_FROM_FILE`.

## Low-level graphics are separate

Old BaselScript examples also build bar/line/pie charts manually with:

```baselscript
draw tile=rect ...
draw tile=line ...
draw tile=circle ...
draw tile=sector ...
```

Those are valid low-level graphics patterns, but they do not define the high-level CHART
API. Do not mix the two models unless the script intentionally does so.

## Generation rules

- Start a new high-level chart with `CHART_BEGIN`.
- Treat `CHART_BEGIN` as a reset boundary.
- Use only confirmed chart type names.
- Configure state with `CHART_SET` rather than inventing chart variables.
- Use one of the documented data-loading families.
- Add `CHART_BIND` when the chart needs explicit category/series binding.
- Finish with `CHART_DRAW`.
- Do not infer nonexistent commands from naming symmetry.
- Do not invent syntax for `CHART_VALUES` or `CHART_LINE` until canonical examples are added.
- Preserve working aliases such as `CHART_FROM_FILE` and `CHART_LABELS_FROM_ARRAY` when
  editing existing scripts.
- Prefer high-level CHART commands for normal charts and low-level graphics only when the
  user explicitly needs custom drawing behavior.

## Corpus coverage measured in V3

Leading-command occurrences in the 425-script V3 corpus:

```text
CHART_BEGIN              38
CHART_SET               523
CHART_VALUES_FROM_FILE   24
CHART_FROM_FILE           6
CHART_VALUES_FROM_ARRAY   8
CHART_LABELS_FROM_ARRAY   8
CHART_BIND               16
CHART_DRAW               38
```

Formal/runtime chart actions such as `CHART_ARRAY_VALUES`, `CHART_ARRAY_LABELS`,
`CHART_ARRAY_COLORS`, `CHART_COLORS_FROM_ARRAY`, and `CHART_LINE` had weak or zero normal
leading-command coverage in that corpus, so dedicated regression/canonical examples should
be added later.
