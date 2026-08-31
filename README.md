# BaselScript Reference - Clean v1

This is a clean rebuild of the BaselScript AI/reference package.

## Design

The package separates three concerns:

- `language/` - machine-readable validator/export contract
- `semantics/` - source-level usage and verified behavior
- `evidence/` - coverage, removed and unverified constructs

This prevents a machine catalog from being mistaken for a complete programming-language manual.

## Update workflow

1. Update the runtime/validator `.def` files in the BaselScript installation.
2. Regenerate `baselscript-language.json`.
3. Copy the six machine-contract files into `language/` unchanged.
4. Update only the semantic files affected by verified runtime/regression evidence.
5. Run `regression/AI_REFERENCE_TESTS.md`.
6. Do not manually edit the generated JSON.

## Current machine summary

The imported generated JSON reports:

- 247 functions
- 126 actions
- 7 blocks
- 1 SCENE grammar
- 16 condition entries
- `loaded = true`
- no load errors

Audit status: `clean-v1-audited` - automated consistency audit passed; intentionally partial semantic areas remain marked as partial.
