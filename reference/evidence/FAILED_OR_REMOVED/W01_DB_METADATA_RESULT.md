# W01 DB metadata result

Date: 2026-08-30
Status: REMOVED FROM CURRENT TRAINING

The maintainer executed `W01_DB_METADATA.script` on the current BaselScript interpreter.
Execution stopped on:

```baselscript
db_current
```

with runtime output equivalent to:

```text
unknown error db_current db_current in db_current
```

The maintainer also confirmed that `db_current`, `db_path`, and `db_exists` are not commands he
recognizes from the actually used BaselScript language.

Decision:

- `db_current` - not CURRENT;
- `db_path` - not CURRENT;
- `db_exists` - not CURRENT;
- all three are removed from the training copy of `actions.def`;
- W01 is removed from the active regression/test queue;
- do not generate these actions.

If they are ever intentionally restored in the interpreter, they require fresh runtime tests before
being promoted again.
