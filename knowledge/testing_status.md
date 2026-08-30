# BaselScript Testing and Control Flow Status

## Purpose

This file documents CURRENT regression-oriented control-flow behavior and the BaselScript test
infrastructure used to protect language changes.

It complements `language_status.md` and `regression_status.md` with canonical generation rules for
FOR, WHILE, FOREACH, BREAK, CONTINUE, ASSERT, and TEST_RUNNER.

## Stable baseline

The documented pre-SWITCH stable baseline is:

```text
PRE_SWITCH_BASELINE_OK
128 / 128 PASS
```

The SWITCH experiment was rolled back and must not be mixed into the stable language reference.

## FOR

FOR is CURRENT and regression-covered.

Canonical section-based pattern:

```baselscript
for #i=0 to=10 step=1 sec=loop_body

SECTION loop_body
    trace #i
END
```

Existing regression coverage includes normal loops, negative step, nested FOR, IF inside FOR,
FOR inside IF, and nested loop break behavior.

Do not invent `ENDFOR`.

## WHILE

CURRENT syntax:

```baselscript
while <condition> sec=<section>
```

Canonical example:

```baselscript
#i=0
while #i < 5 sec=loop_body
trace "WHILE DONE"

SECTION loop_body
    trace #i
    #i=#i + 1
END
```

Semantics:

- condition is evaluated before each iteration;
- false initial condition executes the body zero times;
- the body is a SECTION target;
- BREAK exits the nearest active loop;
- CONTINUE proceeds to the next WHILE condition evaluation;
- there is no `ENDWHILE` in V1.

The current generic validator recognizes WHILE through `actions.def`; runtime additionally enforces
the complete `condition + sec=` form.

## FOREACH

CURRENT syntax:

```baselscript
foreach #item in #array[] sec=<section>
```

Canonical example:

```baselscript
array name=#names[3] value=(Anna,Peter,Maria)
foreach #name in #names[] sec=show_name

SECTION show_name
    trace #name
END
```

FOREACH V1 intentionally iterates BaselScript array storage.

Confirmed semantics:

- source is a BaselScript `#array[]`;
- the iteration variable is a BaselScript `#` variable;
- empty array executes the body zero times;
- one-element array executes once;
- nested loop combinations are supported;
- BREAK/CONTINUE affect the nearest active loop.

Do not generalize FOREACH V1 into SQL-row, object, map, record, or arbitrary collection iteration.

## BREAK

CURRENT standalone action:

```baselscript
break
```

BREAK terminates the nearest active FOR, WHILE, or FOREACH loop.

A BREAK outside an active loop is a runtime error.

Do not model BREAK as a single global boolean in documentation or AI reasoning; nested-loop work
uses loop context so an inner BREAK does not terminate an outer loop.

## CONTINUE

CURRENT standalone action:

```baselscript
continue
```

CONTINUE skips the rest of the current iteration of the nearest active FOR, WHILE, or FOREACH.

A CONTINUE outside an active loop is a runtime error.

False IF branches must not execute BREAK or CONTINUE contained inside them.

## ASSERT

CURRENT syntax:

```baselscript
assert <condition>
assert <condition> message="text"
```

ASSERT uses the same condition evaluation model as IF.

Expected behavior:

```text
TRUE  -> continue execution with no failure
FALSE -> report ASSERT FAILED, include condition/message context, stop current script/test
```

ASSERT inside an inactive IF branch must not execute.

Use ASSERT to express regression invariants instead of reproducing test logic with ad-hoc message
comparisons where possible.

## TEST_RUNNER

CURRENT syntax:

```baselscript
test_run dir="APPDIR/tests/BaselScript_TEST_REPOSITORY_V1" manifest="manifest.csv" mode=all verbose=0
```

Supported documented modes:

```text
mode=all
mode=runtime
mode=validator
```

Typical repository groups include:

```text
01_runtime_assert
02_runtime_expected_fail
03_validator_contract
90_legacy_manual
99_examples
```

TEST_RUNNER is regression infrastructure. It should not be confused with normal application UI or
user-defined test-function syntax.

## Change discipline

Every language change should follow this sequence:

```text
stable baseline
    -> minimal language change
    -> old regression suite
    -> new positive tests
    -> new negative tests
    -> new stable baseline
```

For a new command or construct, add at least:

- one canonical positive test;
- one malformed/negative validator test when static validation is possible;
- one runtime failure test when failure is runtime-only;
- nesting or interaction tests when control flow is affected.

## CURRENT vs experimental

CURRENT:

```text
IF / ELSEIF / ELSE / ENDIF
FOR
WHILE
FOREACH
BREAK
CONTINUE
ASSERT
TEST_RUNNER
```

Not stable/current:

```text
SWITCH / CASE
SECTION parameters
LOCAL scope
user-defined FUNCTION syntax
TRY / CATCH
RECORD
MODULE / IMPORT
REQUIRE
FAIL
IN / NOT IN syntax
```

## AI generation rules

1. Never generate ENDWHILE or ENDFOREACH.
2. Use `sec=` body targets for WHILE and FOREACH.
3. FOREACH V1 source must be a BaselScript array.
4. BREAK/CONTINUE target the nearest loop only.
5. Do not use BREAK/CONTINUE outside loops.
6. Use ASSERT for test invariants.
7. Treat TEST_RUNNER as infrastructure, not an application-language abstraction layer.
8. Do not revive rolled-back experimental constructs from old notes or scripts.
