# BaselScript Regression Status

## Stable baseline

The stable pre-SWITCH baseline remains:

```text
PRE_SWITCH_BASELINE_OK
128 / 128 PASS
```

## Confirmed control flow

Confirmed:

- `IF / ELSEIF / ELSE / ENDIF`
- nested `IF`
- `FOR`
- `WHILE`
- `FOREACH`
- nested loop usage
- `BREAK`
- `CONTINUE`
- `ASSERT`
- `TEST_RUNNER`

`BREAK` applies to the nearest active loop.

## 2026-08-30 weak-action verification

The maintainer executed the following checks in the current interpreter.

### W02_TRACE_DELAY

PASS.

Confirmed:

```text
trace_full
trace_as_is
trace_asis
delay
```

The maintainer observed the actual pause produced by `delay`.

### W03_HASH_PARSING_REMOVE

PASS after correcting the test to use the confirmed `hash_array=1`.

Observed output:

```text
2
3
2
```

This confirms:

- parsed value `b=2`
- hash length before removal = 3
- hash length after removal = 2

### W04_UPDATE_ALL_RECORDS

PASS.

Observed:

```text
W04 OK=1 RECORDS=2 AGE11=1 AGE21=1
```

### W05_REPLACE_FILE

PASS.

Observed:

```text
W05 OK=1 VALUE=OMEGA
```

### M02_SEARCH_DIR

PASS on Windows.

A valid selected directory was returned in `#_SELECTED_DIRECTORY`.

### M03_THUMBNAIL

PASS.

The action completed and the expected output PNG was physically created in the thumbnails directory.

### M04_MESSAGE_SHORT_ECHO

PASS.

Confirmed:

```text
message_short
echo
```

## Removed-action findings

### Database metadata names

`db_current` produced an `unknown error` in the current interpreter.

Decision:

```text
db_current
db_path
db_exists
```

are REMOVED / DO-NOT-GENERATE.

### open

The manual `open` test produced an `unknown error`.

Decision: standalone `open` is REMOVED / DO-NOT-GENERATE.

Use the confirmed standalone `select` file-picker workflow instead.

### Other maintainer removals

The maintainer confirmed these are not CURRENT standalone BaselScript actions:

```text
grid
pdf
mail
vibrate
```

They are absent from the current `actions.def`.

## Dynamic MENU validator regression

Dynamic MENU remains CURRENT runtime syntax:

```baselscript
clear menu=m2
create tile=title text="menu 2"
create tile=item1 text="option" sec=e11
call menu=m2
```

However, the maintainer's installed validator build currently reports:

```text
MENU 'm2' does not exist in SCENE '1'
```

for `R03_DYNAMIC_MENU_REAL_OK.script`.

Status:

```text
Language/runtime - CURRENT
Validator        - OPEN BUG / build synchronization issue
```

Do not downgrade dynamic MENU language status because of this validator defect.

## Missing SECTION/ACTION negative test

`C05_CALL_MISSING_SECTION_BAD.script` correctly produces a validator error for a missing callable
target. This is an expected PASS of a negative test.

## Standalone SELECT

Confirmed CURRENT:

```baselscript
select
```

Windows:
- native file picker opens

Android:
- system document picker opens
- selected URI is copied into BaselScript-accessible temporary storage
- `#_SELECTED_FILE` and `#_SELECTED_DIRECTORY` are populated
- execution resumes after selection

## Not stable CURRENT

Do not treat these as stable language features:

- `SWITCH / CASE`
- SECTION parameters
- LOCAL scope
- user-defined FUNCTION syntax
- TRY / CATCH

## AI interpretation rule

Use:

```text
confirmed current runtime/regression
> current .def
> generated baselscript-language.json
> current knowledge
> repeated real corpus
> rare corpus
> historical examples
```
