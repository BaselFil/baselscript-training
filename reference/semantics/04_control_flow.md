# 04 - Control flow

Status: MOSTLY VERIFIED

Primary machine sources:

- `language/conditions.def`
- `language/actions.def`

## IF family

Current condition keywords:

```text
if
elseif
else
endif
```

`if` and `elseif` require an expression.

Verified form:

```baselscript
if #a == #b
    message "equal"
elseif #a > #b
    message "greater"
else
    message "other"
endif
```

Current semantic operators:

```text
== != > < >= <= contains >< <> && & ||
```

Do not use `=` as equality comparison.

Nested IF is CURRENT and regression-confirmed.

## RETURN

`return` is a CURRENT action and is widely used to leave the current SECTION/ACTION early.

Example:

```baselscript
if #name == ""
    message "Name is required"
    return
endif
```

## FOR

FOR is CURRENT and regression-confirmed.

Important contract detail:

`actions.def` currently recognizes `for`, but does not export one complete canonical source grammar for all FOR forms.

Therefore do not normalize every FOR into one guessed spelling.

A documented working shape is:

```baselscript
for #index=0 step=1 to=9 section=sum
```

Existing runtime/project code also contains legacy/current target aliases. When editing an existing script, preserve its confirmed working FOR target spelling.

Generation rule:

- use a regression-confirmed or project-confirmed FOR pattern;
- preserve existing FOR syntax when modifying code;
- do not invent C/Java/Python loop syntax;
- do not invent `endfor`.

## FOREACH

The current confirmed form is:

```baselscript
foreach #item in #array[] sec=<section>
```

Each iteration assigns the current array element to `#item` and executes the target section.

Do not invent `endforeach`.

## WHILE

The current confirmed form is:

```baselscript
while <condition> sec=<section>
```

The condition is evaluated before the first iteration and again before each next iteration.

Do not invent braces or `endwhile`.

## BREAK / CONTINUE

`break` and `continue` are CURRENT and regression-confirmed.

Confirmed rule:

```text
BREAK    -> nearest active loop
CONTINUE -> nearest active loop
```

An inner BREAK must not terminate an outer loop.

Do not use BREAK or CONTINUE outside an active loop.

CALL interactions preserve loop context in confirmed regression cases, but do not assume arbitrary cross-CALL propagation without a matching regression test.

## Generation rules

- Prefer the documented BaselScript control constructs.
- Keep WHILE/FOREACH work in their section targets.
- Preserve confirmed FOR syntax rather than normalizing it by analogy.
- Do not invent `then`, braces, `endfor`, `endwhile`, `switch`, `case`, `try`, or `catch`.
- `SWITCH / CASE`, `TRY / CATCH`, SECTION parameters, LOCAL scope and user-defined FUNCTION syntax are not stable CURRENT language features.
