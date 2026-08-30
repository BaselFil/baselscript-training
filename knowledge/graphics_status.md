# BaselScript Graphics Reference

This file documents the supported BaselScript graphics syntax.

Use only the forms described here.
Do not invent graphic commands or parameters.

## Graphic scene

Graphics are drawn in a scene with:

```baselscript
mode=graphic
```

Typical structure:

```baselscript
SCENE=1 name="graphics" mode=graphic

SECTION init

    clear canvas=1

    // draw commands

    draw canvas=1

END

END SCENE
```

`clear canvas=1` clears the drawing surface.

Individual graphic objects are added with:

```baselscript
draw tile=<type> ...
```

After the objects have been defined, use:

```baselscript
draw canvas=1
```

to display the result.

## Rectangle

Supported tile:

```text
rect
```

Parameters used by the graphic rectangle include:

```text
x1
y1
x2
y2
w
h
id
fil
c
sw
tra
sec
```

Example:

```baselscript
SCENE=1 name="draw rectangle" mode=graphic

SECTION init

    clear canvas=1

    draw tile=rect id=r1 x1=100 y1=100 w=300 h=200 c=#ff0000 fil=1

    draw canvas=1

END

END SCENE
```

`w` is width.

`h` is height.

`fil=1` requests a filled shape.

`c` specifies the color.

## Circle

Supported tile:

```text
circle
```

The circle uses a center position and radius.

Important parameters:

```text
x1
y1
rad
id
fil
c
sw
tra
sec
```

Example:

```baselscript
SCENE=1 name="draw circle" mode=graphic

SECTION init

    clear canvas=1

    draw tile=circle id=c1 x1=300 y1=300 rad=100 c=blue fil=1 sw=5

    draw canvas=1

END

END SCENE
```

`x1` and `y1` specify the circle center.

`rad` specifies the radius.

## Ellipse

Supported tile:

```text
ellipse
```

An ellipse is defined by its center, width and height.

Parameters include:

```text
x1
y1
x2
y2
w
h
id
fil
c
sw
tra
sec
```

Example:

```baselscript
SCENE=1 name="draw ellipse" mode=graphic

SECTION init

    clear canvas=1

    draw tile=ellipse id=e1 x1=300 y1=250 w=400 h=180 c=blue fil=1 sw=5

    draw canvas=1

END

END SCENE
```

For an ellipse:

```text
x1 = horizontal center
y1 = vertical center
w  = width
h  = height
```

To draw an oval, use `tile=ellipse` with different values for `w` and `h`.

Example:

```baselscript
draw tile=ellipse id=e1 x1=300 y1=250 w=400 h=180 c=blue fil=1
```

## Line

Supported tile:

```text
line
```

A line is defined by its start and end coordinates.

Example:

```baselscript
SCENE=1 name="draw line" mode=graphic

SECTION init

    clear canvas=1

    draw tile=line id=l1 x1=100 y1=100 x2=500 y2=300 c=red sw=5

    draw canvas=1

END

END SCENE
```

Coordinates:

```text
x1 y1 = start
x2 y2 = end
```

## Point

Supported tile:

```text
point
```

Example:

```baselscript
draw tile=point id=p1 x1=300 y1=300 c=blue sw=20
```

## Text

Supported tile:

```text
text
```

Example:

```baselscript
SCENE=1 name="draw text" mode=graphic

SECTION init

    clear canvas=1

    draw tile=text id=t1 x1=100 y1=100 c=blue s=40 text="BaselScript"

    draw canvas=1

END

END SCENE
```

Important parameters:

```text
x1
y1
id
text
s
c
sty
```

## Generation rules for AI

When generating BaselScript graphics:

- use `mode=graphic`
- clear the canvas before creating a new drawing when appropriate
- use `draw tile=<type>` for graphic objects
- use `draw canvas=1` after defining the objects
- use `tile=ellipse` for an oval
- use `tile=circle` only when a circle is requested
- do not invent commands such as `DrawOval`, `Oval`, `DrawEllipse` or `rectangle()`
- do not translate graphic primitives into another programming language
- if the requested positioning cannot be determined from documented screen dimensions or variables, ask for the required dimensions instead of inventing them
