# LIST row and column semantics

Status: VERIFIED BY RUNTIME / UI TEST

This file extends `reference/semantics/05_ui.md` with verified LIST layout behavior.

## Record layout model

For a file-backed LIST, the declared `row` / `col` structure is repeated for every
record of the source file.

Within one record:

- `row` defines the vertical rows of that record.
- `col` defines horizontal fields inside a particular row.
- `w` defines the explicit width of an item.

Canonical verified example:

```baselscript
LIST person_list

    tile=file name=persons directory=#_directory_temp

    tile=item row=1 col=1 text=#first_name w=280 st=bold c=#000000
    tile=item row=1 col=2 text=#last_name w=280 st=bold c=#000000

    tile=item row=2 col=1 text=#city w=280 c=#555555
    tile=item row=2 col=2 text=#birthday w=280 c=#555555

    tile=item row=3 col=1 text=#street w=600 c=#777777

    tile=button id=back_button text="<" sec=back w=160
    tile=select sec=selected_person

END
```

The same row/column structure is repeated automatically for each source record.

## Column alignment across rows

`col=1` and `col=2` do not by themselves guarantee identical visual column widths
across different `row` values.

If columns in several rows must align visually, use the same explicit `w` values
for the corresponding columns.

Preferred:

```baselscript
tile=item row=1 col=1 text=#first_name w=280
tile=item row=1 col=2 text=#last_name w=280

tile=item row=2 col=1 text=#city w=280
tile=item row=2 col=2 text=#birthday w=280
```

Do not rely only on the `col` number for cross-row alignment.

## One-line text versus real columns

If independent values must occupy separate visual columns, use `row` / `col`.

A concatenated one-line item is a different pattern:

```baselscript
#person_line=$concat( \
    #first_name," ",#last_name," | ", \
    #city," | ",#street," | ",#birthday)
```

This produces one textual item, not a column layout.

## Selection and buttons

- `tile=select` selects the current LIST record and can route with `sec=...`.
- Buttons can belong to the LIST structure independently of the record data.

Example:

```baselscript
tile=button id=back_button text="<" sec=back w=160
tile=select sec=selected_person
```

## AI generation rules

- Treat `row` as vertical structure inside one LIST record.
- Treat `col` as horizontal structure inside a row.
- Expect the same row/column structure to repeat for every source record.
- Use explicit matching `w` values when columns must align across rows.
- Prefer `row` / `col` over concatenated text when true columns are requested.
- Do not invent alternative LIST column APIs.
