# Acceptance test 01 - Current weekday name

Result: PARTIAL PASS

A clean GPT generated the correct BaselScript:

```baselscript
#d=$date()
message #_current_weekday_name
```

Semantic result: PASS.

Reference navigation result: FAIL before this patch.

The model attempted repository-root paths such as:

```text
language/functions.def
language/actions.def
```

and received 404 because the actual files are under:

```text
reference/language/
```

This patch makes every manifest path explicitly repository-root-relative and preserves the leading `reference/` directory.
