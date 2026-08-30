# BaselScript Script and Meta Operations Status

## Purpose

This file documents BaselScript commands that manipulate scripts/files as data, generate service
structures, rewrite content, merge files, or otherwise operate on application source and metadata.

These commands are powerful because they can modify program artifacts. AI should preserve source
files and use explicit temp/output targets when the existing pattern provides them.

## REWRITE

`rewrite` is formally CURRENT and has real corpus usage.

Observed forms include:

```baselscript
rewrite file=_assistant.script to_file=_assistant_temp.script sec=modi_record
```

```baselscript
rewrite from_file=example_person to_file=person2 section=skip_record
```

```baselscript
rewrite from_file=notes to_file=notes_new section=update_record encripting=yes
```

The rewrite operation delegates record/content transformation to a SECTION/ACTION callback in
existing patterns.

Do not infer that all historical parameter spellings are equivalent. Preserve the working form used
by the surrounding script.

## REWRITE_ALL_SCRIPTS

`rewrite_all_scripts` is formally CURRENT but rare.

Observed corpus form:

```baselscript
rewrite_all_scripts from_dir=APPDIR/MYDATA/temp from_file=container_EXAMPLE to_dir=APPDIR/MYDATA/out
```

Because it can affect many scripts, generate it only when the user explicitly intends batch source
transformation.

## MERGE

`merge` is formally CURRENT.

Observed form:

```baselscript
merge f1=example_cars f2=example_cars f3=merge_cars
```

Do not generalize argument numbering or destination semantics beyond tested examples.

## MERGE_ALL_SCRIPTS

`merge_all_scripts` is formally CURRENT and rare.

Observed form:

```baselscript
merge_all_scripts dir=#DIR file=list_all_script
```

Treat it as a batch/meta operation, not a normal application control-flow primitive.

## PREFORMAT

`preformat` is formally CURRENT and used by script browsing/editing tools.

Observed patterns:

```baselscript
preformat script=#script
```

```baselscript
preformat script=#selected_script from_directory=#selected_directory output=temp directory=APPDIR/MYDATA/temp
```

```baselscript
preformat script=#selected_script from_dir=#selected_directory output=temp dir=APPDIR/MYDATA/temp
```

Long and short directory aliases occur in historical corpus. Preserve project-local style unless the
runtime contract is being normalized deliberately.

## CREATE_SERVICE

`create_service` is formally CURRENT but rare.

Observed form:

```baselscript
create_service file=#_table_selected hash_array=1
```

Because corpus evidence is sparse, do not invent additional service-generation flags.

## RUN as an evaluator

Some developer/test scripts use:

```baselscript
run from=#function to=#result
```

This can be useful in function test utilities, but `run` is overloaded elsewhere for external
program launch. Do not conflate the two modes.

## File-editing primitives

The current action list also contains low-level text/source operations:

```text
insert_after
insert_before
insert_line_after
insert_line_before
remove
replace
change
add
split
join
```

Their exact parameter contracts are not fully documented in this status file. AI may recognize them
as CURRENT action names, but should not invent syntax without a matching current example or runtime
contract.

## Defined but weakly trained meta/output actions

The formal action list contains additional names with little or no V3 canonical corpus coverage,
including:

```text
parsing
pdf
mail
open
echo
grid
trace_as_is
trace_full
reduce_gups
```

Formal presence means the validator recognizes the action name. It does not justify inventing
parameters or semantics.

Add dedicated positive examples before promoting any weak-contract action into normal AI-generated
patterns.

## Source safety and reversibility

For AI-generated maintenance scripts:

1. Prefer `source -> temp/output` transformations over destructive in-place changes.
2. Keep an original or backup when batch rewriting scripts.
3. Do not change many scripts with `rewrite_all_scripts` or `merge_all_scripts` unless explicitly requested.
4. Validate generated/rewritten scripts before replacing working originals.
5. Run the existing regression suite after language-level or shared-template transformations.

These rules align with BaselScript's broader architectural principle that changes should remain
traceable, testable, and reversible.

## AI generation rules

- Recognize formal action names but do not fabricate missing parameter contracts.
- Use real corpus examples as patterns, not as automatic proof that every historical spelling is CURRENT.
- Keep source and output paths explicit.
- Prefer non-destructive transformations.
- Do not mix script/meta operations with user-defined FUNCTION syntax; user-defined FUNCTION remains non-stable.
