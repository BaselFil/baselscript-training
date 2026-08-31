# BaselScript clean reference audit

Reference version: `clean-v1-audited`

## Result

ALL AUTOMATED CHECKS PASS

## Machine-contract and semantic checks

- PASS - JSON loaded
- PASS - JSON loadErrors empty
- PASS - 247 functions
- PASS - 126 actions
- PASS - 7 blocks
- PASS - 1 scene definition
- PASS - 16 condition entries
- PASS - 12 derived operators
- PASS - CHART prefix order
- PASS - Removed actions absent
- PASS - Condition operators exact
- PASS - No unknown real $function calls in semantics
- PASS - FOR universal-syntax overclaim removed
- PASS - Clipboard exact pattern documented
- PASS - Select result variables documented
- PASS - Line continuation documented
- PASS - test_run/TEST_RUNNER distinction documented

## Corrections made during this audit

1. Added current comment and physical-line normalization semantics to `01_core_language.md`.
2. Removed the unsupported claim that one FOR spelling is universally canonical.
3. Documented nearest-loop BREAK/CONTINUE and exact FOREACH/WHILE forms.
4. Added current `select` result variables and verified Windows/Android picker behavior.
5. Added exact `CLIPBOARD_SET value=#...` pattern and result variables.
6. Added crypto result variables and confirmed SQL_SECURITY access-key forms.
7. Added an explicit no-invention guard for weak `CHART_VALUES` / `CHART_LINE` parameter contracts.
8. Distinguished the BaselScript `test_run` action from the TEST_RUNNER harness name.
9. Added explicit source-authority precedence to `AI_CONTEXT.md`.
10. Extended AI-reference regression prompts.

## Important retained limitations

The following categories remain intentionally partial:

- complete file/data parameter grammar;
- complete array/hash declaration and mutation grammar;
- all UI tile/parameter combinations;
- broad Prefix-family concrete command spellings;
- detailed network/media/device parameter contracts;
- script/meta side effects;
- platform-specific behavior outside the verified select workflow.

Partial means: do not guess.

## Removed actions

The current machine contract intentionally does not contain:

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

Older runtime/checkpoint documents that mention these names do not override the current machine contract.

## Known external issue

Dynamic MENU remains valid CURRENT runtime syntax. A locally installed validator build can still be out of sync and falsely report a runtime-created MENU as missing. That is a validator/build synchronization issue, not a reason to remove dynamic MENU from the language reference.

## Function-call scan

Semantic files contain no unknown non-placeholder `$function(...)` calls.

Unknown calls found:

```text
none
```

## JSON note

`baselscript-language.json -> conditions` already contains all 16 condition-definition rows.
The separate `operators` array is a derived 12-token convenience list and must not be added to the 16 again.
