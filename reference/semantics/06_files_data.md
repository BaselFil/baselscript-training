# File and data semantics

Status: VERIFIED BY CURRENT PROJECT EVIDENCE / REAL SCRIPT PATTERNS

This file documents source-level file declaration and file reading patterns used by
current BaselScript examples.

## File declaration

A structured file can declare the variables that represent one record.

Canonical pattern:

```baselscript
file name=example_person \
    record=(#first_name,#last_name,#city,#street,#birthday) \
    dir=#_directory_files_examples
```

Rules:

- `name=` identifies the BaselScript file definition.
- `record=(...)` maps the fields of one file record to BaselScript variables.
- `dir=` identifies the directory that contains the file.
- If a long statement is split across physical lines, every continued line must end
  with `\`.
- Do not replace `name=` with an undocumented alternative when generating new code.

## Read during scene initialization

When the task is to load file data when a scene starts, place the read operation in
`SECTION init`.

Canonical pattern:

```baselscript
SECTION init
    read file=example_person dir=#_directory_files_examples
END
```

For a file-backed LIST that must be shown immediately after loading, use:

```baselscript
SECTION init
    read file=example_person dir=#_directory_files_examples
    call list=person_list
END
```

Rules:

- `read file=<name> dir=<directory>` reads the declared file.
- `SECTION init` is the normal place for initialization-time file reading.
- Reading the file and declaring the LIST source are different operations.
- Do not generate `draw list=<name>`. A LIST is invoked with `call list=<name>`.

## Complete file-to-LIST pattern

For a task that asks to read `example_person` and show its records in a LIST:

```baselscript
file name=example_person \
    record=(#first_name,#last_name,#city,#street,#birthday) \
    dir=#_directory_files_examples

SCENE=1 title="Example Person"

SECTION init
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

The file declaration defines the record mapping.
`read` loads the file data.
`tile=file` identifies the source used by the LIST.
`call list=person_list` displays the LIST.

## AI generation rules

- For file-backed LIST tasks, combine the `files_data` and `list` routes.
- If the file definition is part of the requested example, include the documented
  `file name=... record=(...) dir=...` declaration.
- Read initialization data inside `SECTION init` when the task requires loading at
  scene start.
- After reading, invoke the LIST with `call list=<name>`.
- Do not confuse `read file=...` with `tile=file ...`.
- Do not invent file identifiers, directories, field mappings, or parameters that
  are not provided by the request or documented reference.
