# BaselScript Language Status

## Purpose

This file defines how AI systems should interpret the current BaselScript language sources.

## Source authority

Use the following precedence when sources disagree:

1. Confirmed regression tests
2. Confirmed post-checkpoint fixes
3. Current `.def` files
4. Repeated usage in the real `.script` corpus
5. Rare corpus usage
6. Review candidates and historical examples

The `.def` files describe intended syntax.
Regression tests describe confirmed behavior.
The real script corpus describes actual usage.

Do not silently promote rare or historical syntax to CURRENT.

## Status classes

### CURRENT

Confirmed CURRENT constructs include:

- `IF / ELSEIF / ELSE / ENDIF`
- nested `IF`
- `FOR`
- `WHILE`
- `FOREACH`
- `BREAK`
- `CONTINUE`
- `ASSERT`
- `TEST_RUNNER`
- `LIST view <name>`
- standalone `select`
- comments handled by the source/exec preprocessing layer

### LEGACY

LEGACY syntax may still occur in older scripts or documentation but should not be generated for new scripts unless explicitly requested.

### EXPERIMENTAL / NOT STABLE

Do not generate these as normal BaselScript syntax unless the user explicitly requests experimental work:

- `SWITCH / CASE`
- SECTION parameters
- LOCAL scope
- user-defined FUNCTION syntax
- TRY / CATCH

### ERROR

Syntax should be treated as ERROR when it is rejected by the current parser, validator, or confirmed runtime behavior.

## Current definition files

- `language/actions.def`
- `language/blocks.def`
- `language/conditions.def`
- `language/functions.def`
- `language/scene.def`
- `language/baselscript-language.json`

When generating BaselScript, consult these files before inventing new syntax.

## Platform notes

The standalone command:

```baselscript
select
```

is CURRENT.

On Windows it opens the native file picker and returns the selected file and directory.

On Android it uses the system document picker. The selected document is made accessible to BaselScript through an internal temporary copy before execution resumes.

Do not assume unrestricted direct access to Android public storage.
