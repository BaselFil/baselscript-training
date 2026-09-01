# Graphics and chart lifecycle patterns

Status: CURRENT GRAPHICS SEMANTICS + REPEATED CORPUS

## Graphics scene lifecycle

A normal graphics pipeline is:

```text
SCENE mode=graphic
-> SECTION init
-> clear canvas
-> draw one or more graphic tiles
-> draw canvas
```

Canonical example:

```baselscript
SCENE=1 name="Graphics" mode=graphic

SECTION init

    clear canvas=1

    draw tile=circle id=c1 x1=300 y1=300 \
        rad=100 c=blue fil=1 sw=5

    draw canvas=1

END

END SCENE
```

Do not omit the final `draw canvas=1` when the documented graphics lifecycle requires
the canvas to be rendered.

Exact graphic tile types and parameters must come from `13_graphics.md`.

## Multiple graphic objects

```baselscript
clear canvas=1

draw tile=line id=l1 x1=50 y1=50 x2=500 y2=50 c=black sw=3
draw tile=rect id=r1 x1=100 y1=120 w=300 h=180 c=red fil=0
draw tile=circle id=c1 x1=500 y1=350 rad=80 c=blue fil=1

draw canvas=1
```

Do not invent a retained-mode object API or methods such as `canvas.addCircle()`.

## File-driven graphics

Many graphics scripts also use FILES/DATA or ARRAYS/FUNCTIONS.

Route-union rule:

```text
graphics from file -> graphics + files_data
graphics from array -> graphics + arrays_hash/functions
```

Prepare the data first, then draw.

## Chart lifecycle

The chart corpus shows a repeated pipeline:

```text
CHART_BEGIN
-> CHART_SET configuration
-> provide values from file/array/direct source
-> optional binding/configuration
-> CHART_DRAW
```

Canonical observed pattern:

```baselscript
CHART_BEGIN type=bars bg=#efefef
CHART_SET title="Daily Values"

CHART_VALUES_FROM_FILE file=#file_name \
    record=(#date:"DATE (dd.mm)",#value:"VALUE (kg)") \
    header=yes

CHART_DRAW
```

The current contract contains distinct actions such as
`CHART_VALUES_FROM_FILE`, `CHART_VALUES_FROM_ARRAY` and `CHART_VALUES`. Use the
specific routed action; do not collapse them into an invented generic chart method.

## Chart from array

When current chart semantics confirms the source form:

```baselscript
CHART_VALUES_FROM_ARRAY #values_array=(100,160,110,180,120,50)
```

Then complete the chart with the documented configuration and `CHART_DRAW`.

## Generation rule

If the user asks for a complete chart example, do not return only `CHART_SET` or only
the values line. Generate the complete current chart pipeline required for rendering.
