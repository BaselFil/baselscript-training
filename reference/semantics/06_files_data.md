# File and data semantics

Status: VERIFIED BY CURRENT RUNTIME / REAL SCRIPT PATTERNS

This file documents structured file declaration, record mapping, reading and
integration with LIST/FORM workflows.

## Structured file declaration belongs inside the SCENE

A structured `file ... record=(...) ...` declaration used by a scene must be inside
that `SCENE`. For normal initialization, place it in `SECTION init`.

The declaration must appear before the corresponding `read file=...`.

Canonical pattern:

```baselscript
SCENE=1 title="Example"

SECTION init

    file name=example_person \
        record=(#first_name,#last_name,#city,#street,#birthday) \
        dir=#_directory_files_examples

    read file=example_person dir=#_directory_files_examples

END

END SCENE 1
```

Do not place `file name=... record=(...) ...` before the `SCENE`.

Rules:

- `file name=... record=(...) dir=...` defines the structured record mapping.
- The order of variables in `record=(...)` follows the field order in the file.
- `read file=...` loads the file after the structure has been declared.
- The file declaration and `read` are separate operations.
- If a long statement is split across physical lines, every continued line
  ends with `\`.

## Complete file-backed LIST lifecycle

Canonical pattern:

```baselscript
SCENE=1 title="Example Person"

SECTION init

    file name=example_person \
        record=(#first_name,#last_name,#city,#street,#birthday) \
        dir=#_directory_files_examples

    read file=example_person dir=#_directory_files_examples

    call list=person_list

END

LIST person_list
    tile=file name=example_person

    tile=item row=1 col=1 text=#first_name w=280
    tile=item row=1 col=2 text=#last_name w=280

    tile=item row=2 col=1 text=#city w=280
    tile=item row=2 col=2 text=#birthday w=280
END

END SCENE 1
```

Responsibility chain:

```text
SECTION init
    file ... record=(...) ...
    -> defines record variables

    read file=...
    -> loads records

    call list=...
    -> displays LIST

LIST
    tile=file ...
    -> identifies LIST source

    tile=item text=#field
    -> displays values from the current record
```

`tile=file` does not replace the structured `file ... record=(...) ...`
declaration.

## Complete file-backed FORM initialization

If a FORM is filled from a structured file, use the same declaration/read order
inside `SECTION init` and draw the form only afterwards:

```baselscript
SCENE=1 title="Person"

SECTION init

    file name=person \
        record=(#first_name,#last_name,#street,#city,#birthday) \
        dir=#_directory_files_examples

    read file=person dir=#_directory_files_examples

    draw form=person_form

END

FORM person_form
    ...
END

END SCENE 1
```

## Writing a record

Status: VERIFIED BY CURRENT RUNTIME / REGRESSION

`write_record` writes the current values of the variables declared by the file's `record=(...)`
mapping. Do not pass a separate values list.

Canonical pattern:

```baselscript
SCENE=1

SECTION init
    file name=person record=(#name,#city) dir=#_directory_files

    #name="Anna"
    #city="Mannheim"
    write_record file=person dir=#_directory_files
END

END SCENE
```

Rules:

- assign the record variables before `write_record`;
- `write_record file=<file>` uses the declared record-variable order;
- `dir=` / `directory=` may be supplied when required by the file location;
- current runtime also handles file options such as delimiter/encryption where documented;
- do not generate `values=(...)` or `fields=(...)` for `write_record`; they are not parameters of the
  current `write_record` runtime path.

For new structured file declarations, prefer `file name=<file> record=(...)`. The current runtime
accepts historical declaration aliases such as `id=`, but new generated examples should use `name=`.

## AI generation rules

- Put `file ... record=(...) ...` inside the `SCENE`.
- For normal initialization, put it inside `SECTION init`.
- Declare the structured file before `read`.
- Do not use record variables in a standalone example without declaring their
  record structure.
- For a file-backed LIST, load both `files_data` and `list` routes.
- For a FORM that reads structured file data, load both `files_data` and `form`.
- Do not confuse `file`, `read`, `tile=file`, `call list`, and `draw form`.
- Do not place a current generated file declaration outside the SCENE.
