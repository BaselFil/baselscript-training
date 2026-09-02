# Files and structured-data lifecycle patterns

Status: STRONGLY CORPUS-GROUNDED / CURRENT SEMANTICS REQUIRED

File/data operations are widespread in the corpus:

```text
FILES/DATA tagged scripts: 201
file declarations:         239 observed command uses
read:                      229 observed command uses
save:                       79 observed command uses
delete:                     58 observed command uses
write_record:               51 observed command uses
copy:                       49 observed command uses
```

The important AI rule is to preserve the lifecycle, not merely individual commands.

## Structured file declaration

In a complete scene-local example, place this declaration inside the relevant SCENE, normally inside `SECTION init`.

A structured file declaration defines the record variables that later file operations
and LIST items use.

Declaration fragment (shown without surrounding SCENE only to explain the declaration itself):

```baselscript
file name=example_person \
    record=(#first_name,#last_name,#city,#street,#birthday) \
    dir=#_directory_files_examples
```

In generated complete code, place this fragment inside the relevant SCENE, normally in
`SECTION init`, as shown in the lifecycle below.

Do not use record variables in a standalone example without defining where they come
from.

## Read lifecycle

```baselscript
SCENE=1

SECTION init
    file name=example_person \
        record=(#first_name,#last_name,#city,#street,#birthday) \
        dir=#_directory_files_examples

    read file=example_person dir=#_directory_files_examples
    message #_counter_of_records
END

END SCENE
```

The current file semantics define the side effects of `read`, including record count
and field-array behavior. Do not invent array names from a convention unless the
semantic file confirms them.

## File-backed LIST

Canonical responsibility chain:

```text
file declaration
    -> defines record variables and directory

read
    -> loads records

call list
    -> activates LIST

tile=file
    -> identifies the LIST source

tile=item
    -> maps record variables to visible fields

tile=select
    -> selects the current record and routes to logic
```

This pattern requires both `files_data` and `list` routes.

## Append/write record

Current scripts use `write_record` heavily. The field/value order must match the
declared record structure.

Canonical complete pattern:

```baselscript
SCENE=1

SECTION init
    file name=languages \
        record=(#data_key,#data_eng,#data_ger) \
        dir=#_directory_files

    #data_key="lan"
    #data_eng="language"
    #data_ger="Sprache"
    write_record file=languages dir=#_directory_files
END

END SCENE
```

`write_record` takes the current values of the declared record variables. Do not generate
`values=(...)` or `fields=(...)` for the current runtime path.

## Delete selected LIST record

A common complete lifecycle is:

```text
read file
-> show LIST
-> tile=select
-> SECTION receives selected record
-> delete record=selected file=...
-> read/reopen LIST
```

Verified source form used by current examples:

```baselscript
delete record=selected file=example_person \
    dir=#_directory_files_examples
```

Do not assume that deleting the visual row alone changes the source file.

## Conditional file processing

The corpus contains callback/section-based file processing. Exact actions and callback
variables differ by operation.

Generation rule:

1. load `06_files_data.md`;
2. declare the source and destination record structures when structured records are
   involved;
3. use the documented callback section;
4. set only the callback control variables that the semantic file confirms.

## Copy / rename / rewrite

These operations are represented in real scripts, but multiple historical aliases and
parameter spellings occur.

For new code:

- prefer the canonical source form documented by `06_files_data.md`;
- do not choose a spelling only because it is frequent in old scripts;
- when modifying a working historical script, preserve accepted spelling unless the
  task is specifically a modernization.

## File picker

Current file selection is a separate lifecycle:

```baselscript
select
```

Then use:

```text
#_SELECTED_FILE
#_SELECTED_DIRECTORY
```

Do not reintroduce the removed standalone `open` action.
