# LIST row and column semantics

Status: VERIFIED BY RUNTIME / UI TEST

For a file-backed LIST, the declared `row` / `col` structure is repeated for every
record of the source file.

Within one record:

- `row` defines the vertical line of the record.
- `col` defines horizontal items inside that row.
- `w` defines the explicit width of an item.
- Different rows may contain different numbers of columns.
- `row` / `col` describe local layout inside one record. They do not define a rigid table grid for the whole LIST.

Example:

```baselscript
LIST person_list

    tile=file name=example_person

    tile=item row=1 col=1 text=#first_name w=280
    tile=item row=1 col=2 text=#last_name w=280

    tile=item row=2 col=1 text=#street w=600

    tile=item row=3 col=1 text=#city w=300
    tile=item row=3 col=2 text=#birthday w=300

END
```

Interpretation:

```text
row 1: | first_name | last_name |
row 2: | street                 |
row 3: | city       | birthday  |
```

The same structure is repeated automatically for every source record.


## Mixed row and column layout

LIST records may use a mixed layout.

Most fields may remain vertically arranged, while selected fields can share
one horizontal row.

Use the same `row` value for fields that must appear next to each other.

Use different `col` values inside that row.

Example:

```baselscript
tile=item row=1 col=1 text=#first_name w=280
tile=item row=1 col=2 text=#last_name w=280

tile=item row=2 col=1 text=#street w=600

tile=item row=3 col=1 text=#city w=600

tile=item row=4 col=1 text=#birthday w=600
```

Rules:

- Items with the same `row` are placed on the same horizontal line.
- Different `col` values place items next to each other within that line.
- Fields that should appear normally one below another use successive `row`
  values, normally with `col=1`.
- Different rows may contain different numbers of columns.
- Do not force every row to use the same column count.
- Do not infer that because one row has two columns, all following rows must also have two columns.
- Do not automatically split fields into several rows unless the requested layout requires it.


## Preferred layout strategy

Use `row` / `col` when the visual structure of individual fields matters.

Columns allow independent control of:

- field width;
- alignment;
- style;
- horizontal grouping;
- mixed single-column and multi-column rows.

Prefer `row` / `col` when fields should remain visually independent.

Example:

```baselscript
tile=item row=1 col=1 text=#first_name w=280
tile=item row=1 col=2 text=#last_name w=280

tile=item row=2 col=1 text=#street w=600
```

This keeps `#first_name` and `#last_name` as two separate display items.

Use concatenation only when several source values are intended to become one textual display value.


## One-line text versus real columns

There are two different patterns for showing several source fields on one line.

### Separate visual fields

Use `row` / `col`:

```baselscript
tile=item row=1 col=1 text=#first_name w=280
tile=item row=1 col=2 text=#last_name w=280
```

This creates two independent LIST items.

Each item may have its own:

- width;
- style;
- alignment;
- formatting;
- later layout behavior.

This is the preferred solution when formatting control is important.


### One combined textual value

If several source fields should become one text value, concatenate them in an
additional SECTION into a working field.

Example:

```baselscript
LIST person_list

    tile=file name=example_person

    tile=item id=#person_line sec=make_person_line

END

SECTION make_person_line

    #person_line=$concat(#first_name," ",#last_name)

END
```

Data flow:

```text
#first_name + #last_name
        |
        v
SECTION make_person_line
        |
        v
#person_line
        |
        v
tile=item
```

Rules:

- Keep the original file fields unchanged.
- Perform concatenation in an additional SECTION.
- Store the result in a working field.
- Display the working field through one `tile=item`.
- Do not invent inline concatenation syntax inside `tile=item`.
- Use this pattern only when the result should behave as one textual value.

Decision rule:

```text
Need independent fields and better layout control
-> use row / col

Need one combined text value
-> SECTION + $concat + working field
```


## Column alignment across rows

`col=1`, `col=2`, and other column numbers do not by themselves guarantee
identical visual widths across different rows.

If columns in several rows must align visually, use matching explicit `w`
values.

Example:

```baselscript
tile=item row=1 col=1 text=#first_name w=280
tile=item row=1 col=2 text=#last_name w=280

tile=item row=2 col=1 text=#city w=280
tile=item row=2 col=2 text=#birthday w=280
```

Do not rely only on the `col` number for cross-row visual alignment.


## File declaration before reading

For a file-backed LIST, the file must be declared inside the SCENE before it is
read.

The normal initialization location is `SECTION init`.

Canonical form:

```baselscript
SCENE=1 title="Person"

SECTION init

    file name=example_person \
        record=(#first_name,#last_name,#street,#city,#birthday) \
        dir=#_directory_files_examples

    read file=example_person
    call list=person_list

END

LIST person_list

    tile=file name=example_person

    tile=item row=1 col=1 text=#first_name w=280
    tile=item row=1 col=2 text=#last_name w=280

    tile=item row=2 col=1 text=#street w=600

    tile=item row=3 col=1 text=#city w=300
    tile=item row=3 col=2 text=#birthday w=300

END

END SCENE
```

Required order:

```text
SCENE
    SECTION init
        file declaration
        read
        call list
    END

    LIST
        tile=file
        tile=item ...
    END
END SCENE
```

Rules:

- The `file` declaration belongs inside the SCENE.
- For normal initialization, place the declaration inside `SECTION init`.
- The file declaration must appear before the corresponding `read file=...`.
- `read` operates on an already declared file.
- `call list=...` normally comes after declaration and reading.
- `tile=file` inside the LIST identifies the LIST data source.
- `tile=file` does not replace the file declaration.


## Invalid file declaration placement

Do not generate a scene-local file declaration before `SCENE`.

Wrong:

```baselscript
file name=example_person \
    record=(#first_name,#last_name,#street,#city,#birthday) \
    dir=#_directory_files_examples

SCENE=1 title="Person"

SECTION init
    read file=example_person
    call list=person_list
END

END SCENE
```

Do not read the file before declaring it.

Wrong:

```baselscript
SCENE=1 title="Person"

SECTION init

    read file=example_person

    file name=example_person \
        record=(#first_name,#last_name,#street,#city,#birthday) \
        dir=#_directory_files_examples

    call list=person_list

END

END SCENE
```

Correct:

```baselscript
SECTION init

    file name=example_person \
        record=(#first_name,#last_name,#street,#city,#birthday) \
        dir=#_directory_files_examples

    read file=example_person
    call list=person_list

END
```


## Static LIST lifecycle

A static LIST is displayed with:

```baselscript
call list=<name>
```

Do not generate:

```baselscript
draw list=<name>
```

Generation rule:

```text
FORM   -> draw form=<name>
LIST   -> call list=<name>
MENU   -> call menu=<name>
DIALOG -> call dialog=<name>
```


## File-backed LIST responsibilities

For a LIST based on a structured file:

```text
file declaration -> defines record variables and directory
read             -> loads file data
call list        -> opens/displays the LIST
tile=file        -> identifies the LIST data source
tile=item        -> defines the visual fields for each record
```

These responsibilities are separate.

Do not merge them into invented syntax.


## AI generation rules

- Treat `row` as the vertical structure inside one LIST record.
- Treat `col` as horizontal structure inside a row.
- Expect the same row/column structure to repeat for every source record.
- Use mixed row/column layouts when appropriate.
- Only fields intended to appear next to each other should share the same `row`.
- Other fields should continue on separate rows.
- Different rows may contain different numbers of columns.
- Do not treat `row` / `col` as a rigid table grid.
- Prefer `row` / `col` when independent field formatting is required.
- Use explicit matching `w` values when columns must align visually across rows.
- Use concatenation only when several source fields should form one textual value.
- Perform concatenation in an additional SECTION and store the result in a working field.
- Do not invent inline concatenation inside `tile=item`.
- Declare the file inside the SCENE before its first use.
- For normal file-backed LIST initialization, declare the file in `SECTION init`.
- Never generate `read file=...` before the corresponding file declaration.
- Do not move a scene-local file declaration before `SCENE`.
- `tile=file` identifies the LIST data source but does not declare the file.
- Display a static LIST with `call list=<name>`.
- Do not generate `draw list=<name>`.
- Do not invent alternative LIST column APIs.
