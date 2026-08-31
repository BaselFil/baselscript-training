# 16 - Script / meta operations

Status: PARTIAL

Current machine-visible actions include:

```text
merge
merge_all_scripts
rewrite
rewrite_all_scripts
reload
run
start
stop
restore
preformat
reduce_gups
create_service
```

These actions can affect scripts, processes, timers, or generated content depending on parameters.

## RUN

Real corpus evidence includes forms such as:

```baselscript
run program=#notepad parameter=#script_full_name
```

and internal transformation/test uses of `run from=... to=...`.

Do not assume these are one universal parameter model; inspect the matching use case.

## RELOAD

Observed forms include:

```baselscript
reload file=_dictionary
reload _global
```

Preserve verified project behavior. Do not invent reload targets.

## MERGE / REWRITE

These are CURRENT names, but their full source/destination/overwrite semantics are not encoded in the machine contract.

Use runtime/project evidence before generating parameters.

## START / STOP

These are general actions also used by timer/notification subsystems.

Execution details belong to the routed subsystem.

## Generation rules

- do not infer destructive behavior;
- do not invent filesystem locations;
- do not infer process execution permissions;
- do not replace BaselScript meta actions with shell commands unless the user explicitly asks for external tooling.
