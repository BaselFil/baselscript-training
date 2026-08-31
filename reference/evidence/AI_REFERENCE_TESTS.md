# AI reference regression tests

These tests verify that an AI actually uses the semantic layer rather than only the machine catalog.

## T01 - Function call marker

Prompt:
`Assign the current date to #d.`

Expected pattern:

```baselscript
#d=$date()
```

Reject:

```text
#d=date()
date()
```

## T02 - Current weekday text

Prompt:
`Print the textual name of the current weekday.`

Expected core:

```baselscript
#d=$date()
message #_current_weekday_name
```

Reject invented helpers such as:

```text
weekday_name()
day_of_week(date())
```

## T03 - Conditions

Prompt:
`Test whether #a equals #b.`

Expected operator:

```text
==
```

Do not use `=` as the semantic comparison operator.

## T04 - Graphics circle

Prompt:
`Draw a circle at x=300 y=300 radius=100.`

Expected to use the verified `draw tile=circle ... x1=... y1=... rad=...` form.

## T05 - Removed actions

Prompt:
`Open a URL with the BaselScript open action.`

Expected behavior:
state that `open` is not in the current machine contract; do not generate it.

## T06 - Undocumented chart parameters

Prompt:
`Generate a complex chart using every CHART_* action.`

Expected behavior:
use only parameter forms that are explicitly documented. Do not invent undocumented parameter layouts.

## T07 - FOR syntax discipline

Prompt:
`Write a FOR loop for an existing BaselScript project.`

Expected behavior:
use a regression-confirmed/project-confirmed FOR spelling and preserve the project's existing target style.

Reject:
invented `for (...) { ... }`, `endfor`, or normalization to a guessed universal syntax.

## T08 - Standalone file selection

Prompt:
`Let the user choose a BaselScript script file.`

Expected core:

```baselscript
select
```

Expected knowledge:
the current workflow exposes `#_SELECTED_FILE` and `#_SELECTED_DIRECTORY`.

Reject:
the removed standalone `open` action.

## T09 - Clipboard

Prompt:
`Copy #edit_password to the clipboard.`

Expected core:

```baselscript
CLIPBOARD_SET value=#edit_password
```

Expected error handling may check:

```text
#_clipboard_result
#_clipboard_error
```

Reject:
invented `clipboard.set(...)`, `$clipboard(...)`, or undocumented parameters.

## T10 - Source continuation

Prompt:
`Format a long CURRENT BaselScript statement across several lines.`

Expected:
prefer the verified trailing `\` continuation form for a new reference example.

Reject:
continuation syntax borrowed from another language.

## T11 - test_run versus TEST_RUNNER

Prompt:
`Run the BaselScript regression suite in validator mode.`

Expected:
use the `test_run` language action with `mode=validator`.

Reject:
invented `TEST_RUNNER ...` as a BaselScript statement.

