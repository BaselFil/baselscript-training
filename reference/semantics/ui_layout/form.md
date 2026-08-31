## Text label height in forms

Status: VERIFIED BY RUNTIME / UI TEST

For a normal one-line `tile=text` label, do not set `height`.

Use the default text-element height.

Preferred:

```baselscript
tile=text x=40 y=80 w=330 text="Vorname"
```

Rules:

- Do not set `height` for a normal one-line `tile=text` label.
- Use explicit `height` only for multiline text or an intentionally fixed text area.
- For multiline labels, use approximately 70 px per text line.
- Do not use an arbitrary small `height` value for one-line labels, because this can change the visual spacing and cause labels to overlap adjacent controls.


## Form width and orientation rules

Status: VERIFIED BY RUNTIME / UI TEST

Form controls should use the available screen width efficiently, but input
fields should not be stretched only because more horizontal space is available.

### Choose one or two form layouts

If the form fits comfortably in portrait orientation, one portrait-oriented
layout may be sufficient.

If the form becomes compressed, difficult to read, or inefficient in one
orientation, define separate layouts for portrait and landscape.

Typical virtual widths:

```text
portrait:   800
landscape: 1280
```

### Portrait layout

For a normal one-column portrait form, use most of the available width while
keeping visible outer margins.

Recommended default:

```text
x = 40..60
w = 680..720
```

Example:

```baselscript
tile=text x=50 y=80 w=700 text="Vorname"
tile=input name=#first_name x=50 y=135 w=700 h=65
```

Do not reduce field width unnecessarily if the form contains only one column.

### Landscape two-column layout

When the form benefits from two columns, distribute the usable width between
the columns.

Recommended pattern for virtual width 1280:

```text
left column:   x=60   w=550
right column:  x=670  w=550
```

Example:

```baselscript
tile=text x=60 y=80 w=550 text="Vorname"
tile=input name=#first_name x=60 y=135 w=550 h=65

tile=text x=670 y=80 w=550 text="Nachname"
tile=input name=#last_name x=670 y=135 w=550 h=65
```

### Landscape one-column layout

Do not automatically stretch a single input field across almost the entire
1280 px landscape width.

Field width should remain appropriate for the expected content.

Recommended default for a normal single-column input:

```text
w = 550..700
```

Long text fields may be wider when this improves usability, but normal fields
such as name, city, phone number or date should not be expanded to approximately
1200 px only because the space is available.

Example:

```baselscript
tile=text x=60 y=80 w=650 text="Vorname"
tile=input name=#first_name x=60 y=135 w=650 h=65
```

Avoid:

```baselscript
tile=input name=#first_name x=40 y=135 w=1200 h=65
```

unless the field is intentionally designed for long content.

### AI generation rules

- Prefer one layout when the form fits comfortably in portrait.
- Create separate portrait and landscape layouts when the form structure
  benefits materially from different arrangements.
- In portrait, prefer one column with approximately 680..720 px field width.
- In landscape, use two columns when this improves the form structure.
- For two landscape columns, use approximately 550 px per column.
- A one-column landscape form must not automatically stretch ordinary fields
  to the full screen width.
- Choose field width according to expected content and layout, not only
  according to available screen width.
- Keep corresponding labels and inputs aligned consistently.