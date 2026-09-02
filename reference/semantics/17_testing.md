# 17 - Testing / diagnostics

Status: STRONGLY GROUNDED FOR CURRENT TEST ACTIONS

Primary source:

- `language/actions.def`
- `language/conditions.def`

Current actions include:

```text
assert
test_run
trace
trace_full
trace_as_is
trace_asis
echo
log
start_diagnose
stop_diagnose
```

## ASSERT

Formal syntax:

```baselscript
assert <condition> [message=<text>]
```

Use condition operators from `conditions.def`.

## TEST_RUN

The BaselScript language action is:

```text
test_run
```

`TEST_RUNNER` is the name of the regression harness/package, not a separate BaselScript keyword.

Formal action syntax:

```baselscript
test_run dir=<path> [manifest=<file>] [mode=<all|runtime|validator>] [verbose=<0|1>]
```

Current modes are:

```text
all
runtime
validator
```

Do not invent unsupported modes.

## TRACE / LOG

These actions provide diagnostic output.

Do not log passwords, encryption keys, protected plaintext, or other secrets.

## Regression model

Reference claims should be classified by evidence:

```text
confirmed runtime/regression
current machine contract
repeated corpus
rare corpus
historical
unverified
```

A regression failure in one local build can indicate validator/runtime synchronization rather than invalid source syntax. Diagnose the layer before changing the language reference.

## AI reference regression

High-value AI tests include:

- function calls retain `$`;
- equality uses `==`;
- current weekday text uses `$date()` + `#_current_weekday_name`;
- removed actions are not generated;
- dynamic UI is not falsely rejected because a static declaration is absent;
- undocumented CHART parameters are not invented.

## Generation rules

- make tests deterministic where possible;
- separate validator-only failure from runtime failure;
- preserve minimal reproductions;
- report expected and actual behavior;
- do not promote a construct to CURRENT from one unconfirmed corpus example.
