# Control-flow lifecycle patterns

Status: CURRENT MACHINE CONTRACT + RUNTIME/REGRESSION CONFIRMED

## IF lifecycle

```baselscript
if #value == 1
    message "one"
elseif #value == 2
    message "two"
else
    message "other"
endif
```

Nested IF is supported. Every IF branch structure closes with `endif`.

## Section-based FOR

The real corpus repeatedly uses a section as the loop body.

Representative pattern:

```baselscript
for #index=0 step=1 to=#counter sec=loop_body

SECTION loop_body
    trace #index
END
```

Use the exact current FOR grammar from `04_control_flow.md` / current action contract.
Do not generate braces or `for(...)`.

## WHILE

Current runtime/regression syntax:

```baselscript
#i=0
while #i < 5 sec=loop_body

SECTION loop_body
    trace #i
    #i=#i + 1
END
```

WHILE has no `ENDWHILE` in the current section-based model.

The condition is evaluated before each iteration.

## FOREACH

Current runtime/regression syntax for BaselScript arrays:

```baselscript
array name=#names[3] value=("Anna","Peter","Maria")

foreach #name in #names[] sec=show_name

SECTION show_name
    trace #name
END
```

FOREACH iterates an existing BaselScript array. Do not generalize it to arbitrary
objects or SQL result sets.

## BREAK and CONTINUE

Current nested-loop rule:

```text
break    -> terminates the nearest active loop
continue -> advances the nearest active loop
```

This applies across current FOR / WHILE / FOREACH nesting.

Do not model BREAK as a global "stop every loop" operation.

## ASSERT

Current regression syntax:

```baselscript
assert #count == 3
assert #name != "" message="name required"
```

Use ASSERT for reproducible expectations, especially around changes to data/control
flow. Do not replace runtime behavior checks with prose comments.

## Composition rule

Loops and IF blocks can be nested, but loop bodies remain section-based where the
current loop action requires a section target.

When generating a complete loop example, include the target SECTION. Do not emit a
standalone line such as:

```text
while #i < 5 sec=loop_body
```

without also defining `SECTION loop_body` in the same runnable example.
