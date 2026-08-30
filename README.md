# BaselScript Training Current

Canonical AI/reference package for the current BaselScript interpreter.

Start here:

```text
BASELSCRIPT_AI_CONTEXT.md
```

Machine entry point:

```text
manifest.json
```

## Repository layout

```text
language/    formal language contract and generated JSON snapshot
knowledge/   semantic and generation rules
regression/  confirmed, negative, and open regression evidence
```

## Interpreter synchronization workflow

The live Windows language contract is stored in:

```text
C:\baselscript\system\language\
```

Files:

```text
actions.def
blocks.def
conditions.def
functions.def
scene.def
baselscript-language.json
```

Workflow:

1. edit the real `.def` files
2. rebuild/run BaselScript
3. let the interpreter regenerate `baselscript-language.json`
4. verify `loaded=true` and empty `loadErrors`
5. copy the six language files into this repository
6. update knowledge/regression only when semantics or confirmed behavior changed

Do not hand-edit `baselscript-language.json`.

## Current snapshot

```text
functions   247
actions     126
blocks      7
scenes      1
conditions  16
```

## Current removed actions

These standalone actions are intentionally absent from the current contract:

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

## GitHub use

The stable URL for AI bootstrap is:

```text
https://raw.githubusercontent.com/BaselFil/baselscript-training/main/manifest.json
```

Old intermediate update ZIPs should not be used as the current reference.
