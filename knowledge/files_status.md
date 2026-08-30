# BaselScript Files and Directories Status

## Purpose

This file documents CURRENT and evidence-backed BaselScript patterns for files, records,
directories, and script/file selection.

The action/function definitions remain authoritative. The corpus supplies usage patterns.
Do not turn historical or singleton spellings into new syntax.

## Core data-file model

Many BaselScript data files are declared with a `file` definition that maps records to
variables, then loaded with `read`.

```baselscript
file id=people record=(#last_name,#first_name,#city) dir=APPDIR/MYDATA/files
read file=people dir=APPDIR/MYDATA/files
```

After a read, record fields may be available as current record values and/or generated
field arrays according to the existing runtime behavior.

## READ

Repeated real forms include:

```baselscript
read file=example_person dir=APPDIR/files/examples
read file=#FILE dir=#DIR
read file=people sort="order by (#last_name)"
read file=people sec=filter
```

Regression-confirmed script-to-string flow also exists:

```baselscript
read script=<script> to_string=<variable>
```

Do not assume every `read` target has the same parameters. Use the form appropriate to the
resource being read.

## WRITE_RECORD

Repeated form:

```baselscript
#name="A"
#city="Mannheim"
write_record file=people dir=APPDIR/MYDATA/files
```

Real applications also contain encrypted record-writing forms. Preserve those only when
working in a script that already uses the corresponding confirmed encryption workflow.

## UPDATE

Observed CURRENT action families include:

```text
update_record
update_current_record
update_records
update_all_records
update_file
```

Representative patterns:

```baselscript
update_current_record file=#FILE dir=#DIR
```

and condition-driven multi-record update:

```baselscript
update_records file=example_person sec=condition
```

Some mutation paths have less regression coverage than simple read/write/delete. When
changing an existing working script, preserve its established call pattern.

## DELETE

File deletion:

```baselscript
delete file=people dir=APPDIR/MYDATA/files
```

Current-record deletion:

```baselscript
delete record=current file=people dir=APPDIR/MYDATA/files
```

Selected-record deletion appears in real scripts:

```baselscript
delete record=selected file=people dir=APPDIR/MYDATA/files
```

Condition-driven delete is regression-confirmed with an important workflow requirement:
load records before the conditional delete.

```baselscript
read file=people dir=APPDIR/MYDATA/files

delete record=on_condition file=people dir=APPDIR/MYDATA/files sec=delete_paris

SECTION delete_paris
    #_delete_record=0
    if #city=="Paris"
        #_delete_record=1
    endif
END
```

Do not omit the preceding `read` when reproducing this confirmed pattern.

## COPY and RENAME

Repeated COPY forms include both one-file and explicit source/destination naming styles.
Examples:

```baselscript
copy file=_mainmenu.script from_directory=APPDIR/script to_directory=APPDIR/MYDATA/script
```

```baselscript
copy from_file=#FILE to_file=#new_name from_dir=#DIR to_dir=#DIR
```

Repeated RENAME forms:

```baselscript
rename file=#FILE to=#new_name dir=#DIR
```

and corpus variants use `to_file=`. Preserve a working local style instead of inventing a
new parameter synonym.

## DIRECTORY_LIST

Strongly repeated pattern:

```baselscript
directory_list dir=#DIR target_file=list
```

Additional real forms include:

```baselscript
directory_list directory=APPDIR/MYDATA/script target_file=dir_list
```

```baselscript
directory_list dir=#DIR target_file=list target_dir=APPDIR/MYDATA/temp mode=files
```

`directory_list` is a prefix-defined action family. Historical `dir_list` forms exist;
prefer the repeated `directory_list` form for new code unless editing a working legacy
script.

## Standalone SELECT

CURRENT standalone command:

```baselscript
select
```

On successful selection, the current import workflow exposes:

```text
#_SELECTED_FILE
#_SELECTED_DIRECTORY
```

Platform behavior differs:

- Windows uses the native file picker.
- Android uses the system document picker and makes the selected document accessible to
  BaselScript through its controlled import/temp workflow.

Do not assume unrestricted Android public-storage access.
Do not confuse standalone `select` with a UI `tile=select`.

## File and directory functions

CURRENT function families in `functions.def` include:

- `file.exist_file` and aliases such as `exist_file`
- `file.counter`
- `file.fields_counter`
- `file.search_in_file`
- `file.max_field`, `file.min_field`, `file.average_field`, `file.sum`
- file timestamps
- `directory.directory_files`
- `exist_subdirectory` and defined aliases
- `get_current_directory`
- `get_higher_directory`

Use the canonical name or an explicitly defined alias. Do not infer aliases from English.

## Corpus-only directory function spellings

The real corpus contains the following spellings, but they are absent from the current
`functions.def` and therefore must NOT be generated as CURRENT syntax without separate
confirmation:

```text
create_directory
copy_directory
delete_directory
rename_directory
```

They are evidence/review candidates, not current language definitions.

## Paths

Prefer logical BaselScript paths already used by the application, for example:

```text
APPDIR/MYDATA/files
APPDIR/MYDATA/script
APPDIR/MYDATA/temp
```

For generated import/download workflows, follow `generation_rules.md`.
Do not hardcode a user's OS-specific Downloads path unless explicitly requested.

## Generation rules

- Declare data-file fields before record-oriented operations when the existing pattern requires it.
- Read before mutation when the confirmed mutation pattern depends on loaded records.
- Preserve `dir=` versus `directory=` style in existing scripts.
- Prefer `directory_list` over rare historical directory-list spellings for new code.
- Use standalone `select` for the current import flow.
- Treat corpus-only directory functions as unverified until added to the language definitions or regression-confirmed.

## Evidence examples

```text
checkpoint/latest_rev19/tests/BATCH11_FILE_CORRECTED/R45C_FILE_DELETE_CONDITION_CYCLE.script
checkpoint/latest_rev19/tests/LOOPS_READ/R27_READ_SCRIPT_TO_STRING_REAL_OK.script
checkpoint/latest_rev19/tests/LIST/L06_DYNAMIC_LIST_FILE_TILE_OK.script
corpus/SERVICE_example_person.script
corpus/10_1_select_file.script
corpus/03_10_directory_list.script
```
