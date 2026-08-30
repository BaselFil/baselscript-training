## Semantic lookup before refusal

A `.def` entry is a contract for name, aliases, arity, and runtime requirement. It is not a complete
description of all semantic effects.

Before answering that BaselScript cannot perform a requested operation:

1. identify the matching task route
2. read its semantic `knowledge/*.md` file
3. inspect the current `.def`
4. check documented system/output variables and side effects
5. check whether CURRENT primitives can be composed into the requested result
6. use confirmed real-script/runtime patterns when available

Do not equate:

```text
"no dedicated one-step function"
```

with:

```text
"unsupported in BaselScript"
```

Example: current weekday text is available through `#_current_weekday_name` after `$date()`, even
though there is no separate function named `weekday_name`.

# BaselScript Generation Rules

## Goal

Generate BaselScript that matches the current interpreter, validator, and verified language definitions.

## Mandatory rules

1. Use only CURRENT syntax unless the user explicitly requests LEGACY or experimental syntax.
2. Do not invent commands, parameters, blocks, functions, conditions, or closing keywords.
3. Check `baselscript-language.json` and the `.def` files before using unfamiliar syntax.
4. Prefer regression-confirmed behavior over examples from old documentation.
5. If two sources disagree, apply the source authority defined in `language_status.md`.
6. If syntax cannot be confirmed, state that it is unverified instead of guessing.
7. Preserve existing working syntax when modifying a script. Change only what is necessary.

## Explicit non-generation list

The following standalone names are not part of the current action contract and must not be generated:

```text
db_current
db_path
db_exists
open
grid
pdf
mail
vibrate
```

Use `select` rather than `open` for the current file-picker workflow.

## Script formatting

Use compact formatting.

Prefer:

```baselscript
SCENE=1 title="Example"

// Initialize the scene
SECTION init
    trace "START"
END

END SCENE
```

Avoid decorative separator comments or oversized banners.

Comments should normally be short English `//` comments placed above SECTIONs or major logical parts.

## Paths

For generated downloadable artifacts, use logical `download/` when a download location is required.

Do not hardcode a user's operating-system-specific Downloads path unless explicitly requested.

For user scripts, the common logical destination is:

```text
APPDIR/MYDATA/script
```

## File import workflow

For importing a generated `.script`, prefer the current standalone `select` workflow:

```baselscript
select
```

After successful selection:

- `#_SELECTED_FILE` contains the selected file name.
- `#_SELECTED_DIRECTORY` contains the directory from which BaselScript can access the selected file.

The current system import script is:

```text
_import_script.script
```

Do not replace this with an older directory-browser workflow unless the user explicitly asks for the legacy example.

## Control flow

Nested `IF` is supported.

Use:

```baselscript
if ...
    ...
elseif ...
    ...
else
    ...
endif
```

Use the current loop constructs only in verified form:

- `FOR`
- `WHILE`
- `FOREACH`
- `BREAK`
- `CONTINUE`

Do not invent closing syntax absent from the current definitions.

## Testing and validation

When producing tests:

- Positive tests should use real confirmed syntax.
- Negative tests should change exactly one thing.
- Keep parser, validator, and runtime failures conceptually separate.
- Do not label something PASS without observed evidence.

Useful metadata:

```baselscript
// TEST: ...
// CHECK: ...
// EXPECT: ...
```

## Uncertain cases

If a construct is absent from the current definitions and regression evidence, classify it as unverified.

Never convert uncertainty into invented BaselScript syntax.
