# BaselScript AI Context

This repository is the authoritative AI reference for the current BaselScript language.

## Repository path rule

All reference paths are relative to the Git repository root.

Correct:

```text
reference/manifest.json
reference/AI_CONTEXT.md
reference/language/actions.def
reference/semantics/05_ui.md
reference/patterns/02_ui_lifecycle.md
```

Do not drop the leading `reference/` directory.

## Source layers

BaselScript AI grounding uses four complementary layers.

### 1. Machine contract

```text
reference/language/*.def
reference/language/baselscript-language.json
```

Defines current names, aliases, arity, action requirements, block tokens, condition
tokens and scene grammar.

It answers primarily: **does this construct exist?**

### 2. Semantics

```text
reference/semantics/*.md
```

Defines verified source forms, meaning, side effects, return/result variables,
platform behavior and domain-specific constraints.

It answers primarily: **what does this construct mean and how is it written?**

### 3. Composition patterns

```text
reference/patterns/*.md
```

Defines verified/repeated end-to-end lifecycles assembled from current constructs.

It answers primarily: **how do these constructs fit together into a working script?**

Patterns never override the current machine contract or semantic constraints.

### 4. Evidence

```text
reference/evidence/*.md
```

Records coverage, removed/legacy/unverified forms, audits and regression status.

## Source authority

When sources disagree, use this precedence:

```text
confirmed current runtime/regression
> current reference/language/*.def
> generated reference/language/baselscript-language.json
> current reference/semantics/*.md
> current reference/patterns/*.md
> repeated real-script evidence
> rare/historical evidence
```

A pattern may compose current semantics but may not resurrect a name intentionally
removed from the machine contract.

## Mandatory loading rule

Before generating, reviewing, explaining, validating or modifying BaselScript code:

1. read `reference/manifest.json`;
2. load every file listed in `baseline_required`;
3. classify the task into every matching `task_routes` category;
4. load the union of all files in all matching routes;
5. use routed composition patterns when the task needs more than an isolated command;
6. only then generate BaselScript.

Do not stop at the first matching route.

Example:

```text
file-backed LIST
-> list + ui + files_data

SQL rows displayed as LIST
-> database_sql + files_data + list + ui

chart from a file
-> charts + files_data
```

## Complete-script rule

When the user requests a complete runnable example, do not return a collection of
individually valid lines if a routed lifecycle pattern exists.

Include the required orchestration.

For example, a file-backed LIST normally requires:

```text
file declaration
-> read
-> call list
-> LIST tile=file
-> LIST tile=item
-> optional tile=select
-> target SECTION
```

A database example normally requires:

```text
db_use
-> check #_database_result
-> application SQL
```

A graphics example normally requires:

```text
graphic scene
-> clear canvas
-> draw tiles
-> draw canvas
```

A chart normally requires:

```text
CHART_BEGIN
-> CHART_SET
-> values source
-> CHART_DRAW
```

## Core lexical rules

- user and system variables use `#`;
- functions use `$`;
- actions are statements and do not use `$`;
- assignment uses `=`;
- equality comparison uses `==`;
- multiline logical statements require explicit `\` continuation.

Example:

```baselscript
#d=$date()
message #_current_weekday_name
```

Do not generate:

```text
date()
$message(...)
```

## Physical-line rule

Parentheses, commas and indentation do not imply physical-line continuation.

Keep reasonably short statements on one physical line.

When splitting a long BaselScript statement, end every continued physical line with
`\`.

```baselscript
message $concat( \
    #first_name," ", \
    #last_name)
```

## Hard generation rule

Do not invent:

- actions;
- functions;
- parameters;
- blocks;
- condition operators;
- closing keywords;
- CALL target families;
- UI tiles;
- graphics tiles;
- CHART syntax;
- system variables;
- platform APIs.

If the current routed reference does not document the required source form, state that
it is unverified instead of substituting syntax from another language.

## Canonical names

Prefer canonical names from the current machine contract. Accepted aliases may be
preserved when modifying an existing working script, but frequency in old corpus files
does not by itself make an alias canonical.

## Removed standalone actions

Do not generate these unless a future current contract explicitly reintroduces them:

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

Use current documented alternatives, for example standalone `select` for file
selection.

## Cross-domain responsibility

A routed semantic or pattern file may explicitly require another route. Follow that
requirement.

Examples:

- a file-backed LIST requires `files_data`;
- SQL output shown in a LIST also requires file/UI semantics;
- file-driven graphics require both graphics and files;
- encrypted database work requires database and security routes.

## "Reference loaded"

Do not claim the BaselScript reference is loaded if only the machine catalog was read.
A task is reference-grounded only after baseline files and every matching routed
semantic/pattern file have been read.

## Current corpus second pass

The composition layer was added after a corpus-wide second pass over the extracted
catalog of 425 real `.script` files, 27,094 executable/structural lines and 1,512
distinct syntactic pattern signatures.

See:

```text
reference/evidence/SECOND_PASS_REPORT.md
```

for methodology and coverage.
