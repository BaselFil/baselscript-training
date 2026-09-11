# BaselScript AI Reference Versioning

`reference/manifest.json` must contain:

```json
"reference_version": "YYYY.MM.DD-N"
```

Example:

```json
"reference_version": "2026.09.11-1"
```

Increment the version whenever a change can alter how an AI generates, validates,
explains, or composes BaselScript.

Update procedure:

1. Edit the authoritative file under `reference/`.
2. Increment `reference_version` in `reference/manifest.json`.
3. Add a short entry to `reference/CHANGELOG_AI.md`.
4. Commit/push.
5. The build workflow regenerates FULL, UPDATE and INDEX files.
