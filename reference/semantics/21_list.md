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

    tile=file name=example_person

    tile=item row=1 col=1 text=#first_name w=280 st=bold
    tile=item row=1 col=2 text=#last_name w=280 st=bold

    tile=item row=2 col=1 text=#city w=280
    tile=item row=2 col=2 text=#birthday w=280

    tile=item row=3 col=1 text=#street w=600

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

## Static LIST lifecycle

Status: VERIFIED BY RUNTIME / REGRESSION / REAL SCRIPTS

A static LIST is displayed with:

```baselscript
call list=<name>
```

Do not generate:

```baselscript
draw list=<name>
```

`draw form=<name>` is used for FORM.
A LIST is invoked with `call list=<name>`.

For a file-backed LIST that should be displayed when the scene initializes:

```baselscript
SECTION init
    read file=example_person dir=#_directory_files_examples
    call list=person_list
END
```

The LIST itself defines the visual structure:

```baselscript
LIST person_list

    tile=file name=example_person

    tile=item row=1 col=1 text=#first_name w=280
    tile=item row=1 col=2 text=#last_name w=280

    tile=item row=2 col=1 text=#city w=280
    tile=item row=2 col=2 text=#birthday w=280

END
```

Generation rule:

```text
FORM   -> draw form=<name>
LIST   -> call list=<name>
MENU   -> call menu=<name>
DIALOG -> call dialog=<name>
```

## File-backed LIST responsibilities

For a list built from a structured file:

```text
file declaration -> defines record variables and directory
read             -> loads file data
call list        -> opens/displays the LIST
tile=file        -> identifies the LIST data source
tile=item        -> defines the visual fields for each record
```

Do not merge these responsibilities into invented syntax.

When the requested LIST is based on a file, the AI must also load the
`files_data` task route.

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
- Display a static LIST with `call list=<name>`.
- Do not generate `draw list=<name>`.
- For a file-backed LIST, load and apply the `files_data` route as well.
- Do not invent alternative LIST column APIs.
