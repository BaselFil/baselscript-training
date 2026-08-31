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