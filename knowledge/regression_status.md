# BaselScript Regression Status

## Stable baseline

The current stable pre-SWITCH baseline is confirmed with:

```text
PRE_SWITCH_BASELINE_OK
128 / 128 PASS
```

## Confirmed control-flow behavior

### IF

Confirmed:

- `IF`
- `ELSEIF`
- `ELSE`
- `ENDIF`
- nested `IF`

### Loops

Confirmed:

- `FOR`
- `WHILE`
- `FOREACH`
- nested loop usage
- `BREAK`
- `CONTINUE`

`BREAK` applies to the nearest active loop.

### Assertions and tests

Confirmed:

- `ASSERT`
- `TEST_RUNNER`
- validator regression coverage

## Validator

Current execution flow:

```text
FILE
  -> sourceLines
  -> RemoveComments()
  -> execLines
  -> Validator
  -> Runtime
```

`sourceLines` remain available for editor/browse/save behavior.
`execLines` are used for validation and execution.
The comment preprocessing step preserves physical line count.

## LIST

Confirmed current form:

```baselscript
LIST view <name>
```

## Standalone SELECT

Confirmed standalone command:

```baselscript
select
```

This is different from LIST syntax such as:

```baselscript
tile=select
```

### Windows

Confirmed:

- parser accepts bare `select`
- native file picker opens

### Android

Confirmed:

- parser accepts bare `select`
- system document picker opens
- selected URI can be read
- selected file is copied to `MYDATA/temp`
- `#_SELECTED_FILE` is populated
- `#_SELECTED_DIRECTORY` is populated
- interpreter resumes after `select`
- selected script can be copied to `APPDIR/MYDATA/script`
- imported script execution is confirmed
- broad all-files access is not required

## Not part of the stable baseline

Do not treat the following as stable CURRENT language features:

- `SWITCH / CASE`
- SECTION parameters
- LOCAL scope
- user-defined FUNCTION syntax
- TRY / CATCH

A previous SWITCH experiment was rolled back.

## AI interpretation rule

Use this order:

```text
regression evidence
> confirmed post-baseline fixes
> current .def files
> repeated real-corpus usage
> rare corpus usage
> historical examples
```

Do not claim a feature is confirmed unless there is actual regression or runtime evidence for it.
