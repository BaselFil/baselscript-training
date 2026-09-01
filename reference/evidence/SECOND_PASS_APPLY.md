# Apply the BaselScript second-pass overlay

This ZIP is an overlay for the repository root.

It adds:

```text
reference/patterns/
reference/evidence/SECOND_PASS_REPORT.md
```

and replaces:

```text
reference/manifest.json
reference/AI_CONTEXT.md
```

The existing `reference/language/` and `reference/semantics/` files are not deleted or
replaced by this package.

Recommended workflow:

1. make a Git commit or backup of the current reference;
2. extract this ZIP into the repository root;
3. verify that every path listed in `reference/manifest.json` exists;
4. keep the current generated `reference/language/baselscript-language.json`;
5. test a fresh chat with multi-domain tasks, especially:
   - file-backed LIST;
   - SQL result displayed as LIST;
   - FORM with event SECTION;
   - chart from file;
   - graphics scene;
   - WHILE/FOREACH with section body.
