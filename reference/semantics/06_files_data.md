# 06 - Files / data

Status: PARTIALLY VERIFIED

Primary sources:

- `language/actions.def`
- `language/functions.def`
- repeated real script patterns

Current statement-level families include:

```text
file
read
write_record
copy
delete
rename
search
select
select_record
update_file
update_records
update_all_records
directory_l*
dir_l*
search_dir*
```

Some file/data constructs are parser families rather than fully specified action rows. Use verified examples.

## File declaration / record mapping

A common real pattern is:

```baselscript
file id=PERSONS_TEMP.csv dir=#_directory_files \
    record=(#id,#first_name,#last_name)
```

The `record=(...)` mapping defines fields loaded from a file row.

## READ

Common form:

```baselscript
read file=<file> dir=<directory>
```

Examples may add sorting or other parameters. Preserve a verified pattern rather than inventing a parameter.

## WRITE_RECORD

`write_record` is CURRENT. Use a real pattern from the target application/file model before generating full parameters.

## Directory listing

Confirmed project pattern:

```baselscript
directory_list dir=APPDIR/script target_file=dir_list mode=only_files
```

A typical pipeline is:

```baselscript
directory_list dir=APPDIR/script target_file=dir_list
read file=dir_list
call list=<name>
```

## File functions

Current machine-defined families include:

```text
file.exist_file
file.counter
file.fields_counter
file.search_in_file
file.max_field
file.min_field
file.average_field
file.sum
file.creation_time
file.last_access_time
file.last_write_time
directory.directory_files
exist_subdirectory
```

Use `$` function syntax and respect arity.

## Selection / picker

Standalone `select` is CURRENT:

```baselscript
select
```

After a successful current picker workflow, BaselScript exposes:

```text
#_SELECTED_FILE
#_SELECTED_DIRECTORY
```

Do not generate removed standalone `open`.

The exact Windows/Android picker and temporary-copy behavior belongs to `18_platform.md`.

## Generation rules

- Follow the application's established `file id=... dir=... record=(...)` model.
- Preserve directory variables and APPDIR/MYDATA conventions when maintaining code.
- Do not infer path semantics from Windows or Android directly.
- Do not generate corpus-only directory functions absent from `functions.def`.
