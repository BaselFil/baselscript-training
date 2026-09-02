# Script and meta-operation lifecycle patterns

Status: CORPUS-GROUNDED, EXACT SYNTAX DEFERRED TO CURRENT SEMANTICS

Script/meta operations are less common than ordinary application flow. Historical
scripts also contain aliases and platform-specific variants.

## Cross-script application flow

For ordinary application navigation, prefer the current CALL layer:

```baselscript
call script=target_script
```

or the routed scene/script variant documented by `03_call_execution.md`.

Do not use lower-level script/meta operations when a normal CALL expresses the task.

## Script generation/editing lifecycle

The corpus contains assistant/template scripts that build script text dynamically.
Their architecture is broadly:

```text
collect requested structure/data
-> create or append script text
-> save script
-> optionally reload/run the generated script
```

This is a specialized meta-programming workflow. Do not apply it to ordinary user
applications.

## Reload / save / rewrite operations

The corpus contains `reload`, script save and rewrite-related operations, but exact
forms vary and some are historical.

For new code:

1. load `16_script_meta.md` and `actions.def`;
2. use the current documented operation;
3. preserve the current script/directory responsibility;
4. do not infer a filesystem or process API from C#/Java/Python.

## External program execution

`run program=...` occurs in historical/desktop scripts and is platform-sensitive.

Do not generate it for a portable BaselScript task unless the current semantic route
explicitly confirms the platform and source form.

## Generation rule

Use script/meta actions only when the user's task is actually about scripts as data,
loading/reloading, generation, or external execution. Normal application transitions
belong to `call script=...`.
