# 13 - Graphics

Status: STRONGLY VERIFIED

Primary sources:

- `language/actions.def`
- `language/functions.def`
- current graphics reference and real corpus

## Graphic scene

Canonical pattern:

```baselscript
SCENE=1 name="graphics" mode=graphic

SECTION init
    clear canvas=1

    // graphic objects

    draw canvas=1
END

END SCENE
```

Individual objects use:

```baselscript
draw tile=<type> ...
```

## Current primitive set

Confirmed current primitives include:

```text
rect
circle
ellipse
line
point
text
arc
sector
segment
polygon
image
property
prop
```

Do not infer new primitive names.

## Circle

```baselscript
draw tile=circle id=c1 x1=300 y1=300 rad=100 c=blue fil=1 sw=5
```

`x1`,`y1` = center. `rad` = radius.

## Ellipse

```baselscript
draw tile=ellipse id=e1 x1=300 y1=250 w=400 h=180 c=blue fil=1 sw=5
```

`x1`,`y1` = center; `w`,`h` = dimensions.

## Line

```baselscript
draw tile=line id=l1 x1=100 y1=100 x2=500 y2=300 c=red sw=5
```

## Point

```baselscript
draw tile=point id=p1 x1=300 y1=300 c=blue sw=20
```

## Text

```baselscript
draw tile=text id=t1 x1=100 y1=100 c=blue s=40 text="BaselScript"
```

## Arc / sector / segment / polygon

Use only verified real patterns. Preserve observed parameter spellings such as radius/start/sweep variants rather than inventing a normalized graphics API.

Example sector:

```baselscript
draw tile=sector id=s1 x1=#x1 y1=#y1 radius=#radius start_angle=#start sweep_angle=#sweep sw=#stroke c=#color
```

Example polygon:

```baselscript
draw tile=polygon id=poly c=#ff0000 fill=1 stroke_width=6 x1=#xx_array y1=#yy_array
```

## Property state

`property` and `prop` set drawing state for subsequent primitives.

Example:

```baselscript
draw tile=property c=#0000ff stroke_width=15 fill=1
```

## Generation rules

- use `mode=graphic` for graphic scenes;
- clear the canvas when starting a new drawing where appropriate;
- add objects with `draw tile=...`;
- finish with `draw canvas=1`;
- do not translate into another graphics API;
- do not assume UI tile=image and graphic draw tile=image share every parameter.
