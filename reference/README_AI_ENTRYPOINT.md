# AI entry point

For AI-assisted BaselScript generation or review, start with:

```text
reference/manifest.json
reference/AI_CONTEXT.md
```

All reference paths are relative to the repository root.

## Mandatory loading scope

Do not load the entire BaselScript reference by default.

For each request:

1. Read `reference/manifest.json`.
2. Load every file listed in `baseline_required`.
3. Determine all matching `task_routes`.
4. Load the union of files from those matching routes.
5. Generate or review BaselScript only after those routed files are available.

Do not load unrelated routes unless they are required by the request.

If the required routed files cannot be loaded, state that the relevant BaselScript reference is
unavailable. Do not invent syntax.

## Path resolution

Examples:

```text
reference/language/functions.def
reference/language/actions.def
reference/semantics/10_date_time.md
```

Do not resolve them as:

```text
language/functions.def
semantics/10_date_time.md
```

## Generation scope

Generate only the functionality requested by the user.

Do not add unrelated features such as edit, delete, search, sorting, autosave, database support,
navigation, or additional screens unless the user requests them.

Do not target or advertise an arbitrary source-code line count.

Generate the smallest complete BaselScript solution that satisfies the request.

## Failure behavior

If `reference/manifest.json`, `baseline_required`, or required routed files cannot be loaded:

- say which required reference content is unavailable
- do not guess
- do not invent BaselScript commands, functions, parameters, blocks, or conditions
- ask for the missing reference only when it is necessary to continue
