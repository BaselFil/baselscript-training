# BaselScript AI Context

This repository is the authoritative AI reference for the current BaselScript language.

## Repository path rule

All paths in `reference/manifest.json` are relative to the Git repository root and retain the
leading `reference/` directory.

## Reference layers

1. `reference/language/` - machine contract: names, aliases, arity, structural tokens and generated metadata.
2. `reference/semantics/` - verified source forms, meaning, side effects and domain constraints.
3. `reference/patterns/` - verified composition/lifecycle patterns.
4. `reference/knowledge/generation_rules.md` - mandatory generation and formatting policy.
5. `reference/evidence/` and `reference/regression/` - verification material, not default prompt context.

## Mandatory loading rule

Before generating, reviewing, explaining, validating or modifying BaselScript:

1. read `reference/manifest.json`;
2. load every file in `baseline_required`;
3. classify every matching `task_routes` category;
4. apply relevant `route_expansion_rules`;
5. load the union of those routed files;
6. generate only after that context is available.

Do not load the whole repository by default. Do not stop at the first matching route.

`reference/language/baselscript-language.json` is deliberately not baseline context. Load it only
when the manifest routes `ui_defaults` or `machine_full`, or when fallback machine-wide validation
is required.

## Complete script rule

For a complete runnable script, include required orchestration. Use the `complete_script` expansion
and every domain route implied by the request.

Examples:

```text
file-backed LIST                  -> files_data + list + complete_script
file-backed FORM                  -> files_data + form + complete_script
adaptive FORM                     -> form + platform + complete_script
adaptive file FORM + LIST         -> files_data + form + list + platform + complete_script
SQL rows displayed as LIST        -> database_sql + files_data + list + complete_script
```

## Source authority

When a conflict remains after routing, prefer:

```text
confirmed current runtime/regression
> current routed machine contract
> current routed semantics
> current routed patterns
> repeated real-script evidence
> rare/historical evidence
```

Domain-specific semantics override a generic example for that domain. Patterns compose semantics;
they do not override them.

## Hard generation rule

Do not invent actions, functions, parameters, blocks, conditions, closing keywords, CALL target
families, UI tiles, system variables, or platform APIs.

If required routed reference content is unavailable, identify what could not be loaded and do not
guess.

## Core lexical reminders

- variables use `#`;
- functions use `$`;
- actions are statements and do not use `$`;
- assignment uses `=`;
- equality uses `==`;
- long logical statements require explicit `\` physical-line continuation.

## Current high-value domain files

```text
FORM   -> reference/semantics/19_form.md
DIALOG -> reference/semantics/20_dialog.md
LIST   -> reference/semantics/21_list.md
files  -> reference/semantics/06_files_data.md
```

For file-backed LIST or FORM work, always load `files_data` in addition to the UI route.

## Canonical spelling rule

When historical examples use a different accepted alias, follow
`reference/knowledge/generation_rules.md` for newly generated code. Regression-confirmed historical
spellings remain valid evidence for existing scripts but do not automatically become the generation
default.
