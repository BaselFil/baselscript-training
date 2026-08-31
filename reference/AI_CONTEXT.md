# BaselScript AI Context

This repository is the authoritative AI reference for the current BaselScript language.

## Repository path rule

All file paths in this reference are relative to the **Git repository root**.

Correct:

```text
reference/manifest.json
reference/AI_CONTEXT.md
reference/language/functions.def
reference/language/actions.def
reference/language/blocks.def
reference/language/conditions.def
reference/language/scene.def
reference/language/baselscript-language.json
reference/semantics/10_date_time.md
```

Wrong:

```text
language/functions.def
language/actions.def
semantics/10_date_time.md
```

Do not remove the leading `reference/` directory when constructing GitHub paths or URLs.

## Source layers

1. `reference/language/*.def` and `reference/language/baselscript-language.json`
   - machine contract used by the validator/reference exporter
   - defines known names, aliases, arity, block tokens, condition tokens and runtime requirements
   - does NOT by itself define every source-level invocation form, return convention, side effect or platform behavior

2. `reference/semantics/*.md`
   - canonical source-level usage and verified behavioral semantics
   - this is where invocation syntax, side effects, compositions and platform notes belong

3. `reference/evidence/*.md`
   - confidence and coverage status
   - unverified, removed and legacy forms must not be generated as current syntax


## Source authority when files disagree

Use this precedence:

```text
confirmed current runtime/regression
> current language/*.def
> generated language/baselscript-language.json
> current semantics/*.md
> repeated real-script evidence
> rare/historical evidence
```

The `.def` files remain the formal current name/arity/token contract.
Semantic files may add verified behavior that the machine contract does not encode, but must not silently reintroduce a name that the current machine contract has intentionally removed.

## Mandatory loading rule

Before generating BaselScript:

1. read `reference/manifest.json`
2. load every file in `baseline_required`
3. classify the request into one or more `task_routes`
4. load every routed semantic file and machine-contract file for those routes
5. generate only syntax supported by those files

If the required source-level form is not documented, do not substitute syntax from C#, JavaScript, Python, SQL dialects or another language.

## Core lexical rules

- BaselScript variables use `#`.
- BaselScript function calls use `$` before the function name.
- Canonical assignment of a function result is therefore of the form:

```baselscript
#result=$function_name(...)
```

Do not generate:

```text
function_name(...)
#result=function_name(...)
```

unless a routed semantic file explicitly documents that form for a special construct.

## Canonical names

Aliases listed in `functions.def` are accepted language spellings, but AI-generated code should prefer the canonical name unless a semantic file documents a preferred public spelling.

## "Reference loaded"

Do not claim that the BaselScript reference is loaded if only the machine catalog was read. For a task to be reference-grounded, the baseline and matching semantic route must both be read.
