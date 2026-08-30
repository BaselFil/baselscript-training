# BaselScript AI Knowledge V2

Status: current Windows reference behavior after NESTED_IF_RUNTIME_V1, LOOP_CONTROL_V1/V2 and WHILE_V1.

Purpose: compact operational knowledge for an AI that generates, reviews or explains BaselScript code.

## 1. General execution model

BaselScript is section-oriented. Many control-flow operations execute named sections instead of using brace-delimited blocks.

A typical script contains a scene, one or more sections, and actions:

```text
SCENE=1

SECTION init
    trace "START"
END

END SCENE
```

When generating BaselScript, prefer existing language forms and section-based execution. Do not invent C/Java/Python syntax.

---

## 2. IF / ELSEIF / ELSE / ENDIF

Nested IF is supported.

Canonical form:

```text
if <condition>
    ...
elseif <condition>
    ...
else
    ...
endif
```

Example:

```text
if #a == 1
    trace "A"
    if #b == 2
        trace "B"
    else
        trace "NOT B"
    endif
else
    trace "NOT A"
endif
```

Supported behavior:

- nested IF blocks;
- ELSEIF inside nested IF;
- ELSE;
- ENDIF;
- false parent branch suppresses evaluation/execution of child branches;
- nested conditions survive section calls and return correctly in confirmed cases;
- legacy non-nested IF action data remains supported by a compatibility path.

Structural errors that must be rejected:

```text
endif
```

without an open IF;

```text
else
```

without an open IF;

```text
elseif ...
```

without an open IF;

multiple ELSE branches in the same IF;

ELSEIF after ELSE;

unclosed IF blocks.

For AI-generated BaselScript:
- always close each IF with ENDIF;
- do not emit more than one ELSE per IF;
- do not emit ELSEIF after ELSE.

---

## 3. FOR

FOR already exists and is section-based.

Existing syntax family uses an index, step, end value and loop-body action/section. Preserve the syntax already used by the target codebase; do not silently rewrite FOR into WHILE.

Confirmed behavior includes:
- positive step;
- negative step;
- nested IF inside FOR;
- FOR inside IF;
- FOR nested inside WHILE;
- WHILE nested inside FOR;
- break;
- continue.

When editing existing FOR code, preserve its section-based execution model.

---

## 4. BREAK

`break` is a standalone action with no parameters.

```text
break
```

Meaning:

- terminates the nearest active loop;
- works in FOR;
- works in WHILE;
- works when placed inside nested IF within a loop;
- an inner BREAK must not terminate an outer loop.

Example:

```text
SECTION loop_body
    if #i == 2
        break
    endif

    trace #i
END
```

Invalid use:

```text
break
```

outside any active loop.

Expected runtime error:

```text
BREAK outside loop.
```

Important semantic rule for AI:
BREAK targets the nearest active loop context, not all loops.

---

## 5. CONTINUE

`continue` is a standalone action with no parameters.

```text
continue
```

Meaning:

- stops the current loop-body execution;
- resumes the nearest active loop;
- in FOR, proceeds to the next iteration;
- in WHILE, returns to condition reevaluation;
- works inside nested IF.

Example:

```text
SECTION loop_body
    #i=#i + 1

    if #i == 2
        continue
    endif

    trace #i
END
```

Invalid use outside a loop produces:

```text
CONTINUE outside loop.
```

Important semantic rule for AI:
CONTINUE targets the nearest active loop context.

---

## 6. WHILE

WHILE is supported.

Canonical V1 syntax:

```text
while <condition> sec=<section>
```

Example:

```text
SCENE=1

SECTION init
    #i=0

    while #i < 4 sec=loop_body

    trace "AFTER WHILE #i="+#i
END

SECTION loop_body
    trace "#i = "+#i
    #i=#i + 1
END

END SCENE
```

Expected final value:

```text
#i = 4
```

WHILE is section-based.

There is no `ENDWHILE` in WHILE_V1.

Do not generate:

```text
while #i < 4
    ...
endwhile
```

unless a future BaselScript version explicitly adds such syntax.

---

## 7. WHILE conditions

The condition is evaluated before every iteration.

Initially false condition:

```text
#i=10
while #i < 3 sec=body
```

does not execute `body`.

Parentheses are supported:

```text
while (#i < 5) sec=body
```

Nested parentheses are supported:

```text
while ((#i < 5)) sec=body
```

Logical expressions with parentheses are supported in confirmed tests:

```text
while ((#i < 5) && (#i < 3)) sec=body
```

When generating conditions, prefer explicit parentheses for compound expressions.

---

## 8. WHILE + BREAK / CONTINUE

BREAK in WHILE:

```text
SECTION body
    #i=#i + 1

    if #i == 3
        break
    endif
END
```

The nearest WHILE terminates.

CONTINUE in WHILE:

```text
SECTION body
    #i=#i + 1

    if #i == 2
        continue
    endif

    trace #i
END
```

CONTINUE returns to WHILE condition evaluation.

The body itself must change state if the condition depends on mutable variables. AI must avoid generating an accidental infinite loop such as:

```text
#i=0
while #i < 5 sec=body

SECTION body
    trace #i
END
```

because `#i` never changes.

---

## 9. Nested loops

Nested loops are supported.

Confirmed combinations:

```text
WHILE -> WHILE
WHILE -> FOR
FOR   -> WHILE
```

Critical rule:

```text
inner break -> exits inner loop only
```

Example pattern:

```text
SECTION outer_body
    #j=0
    while #j < 10 sec=inner_body
    #i=#i + 1
END

SECTION inner_body
    if #j == 1
        break
    endif
    #j=#j + 1
END
```

The outer loop must continue after the inner BREAK.

For AI review:
flag any implementation or generated pattern that models BREAK as one global "stop all loops" switch.

---

## 10. IF inside loops

Supported:

```text
while #i < 5 sec=body

SECTION body
    if #i == 2
        continue
    endif

    if #i == 4
        break
    endif

    #i=#i + 1
END
```

Nested IF context and loop context are independent.

An inactive IF branch must not execute BREAK or CONTINUE.

---

## 11. Loops inside IF

Supported:

```text
if #enabled == 1
    while #i < 3 sec=body
endif
```

Also supported in the other direction:

```text
while #i < 3 sec=body

SECTION body
    if #flag == 1
        trace "ACTIVE"
    endif
END
```

---

## 12. WHILE syntax diagnostics

Malformed WHILE without a condition:

```text
while sec=body
```

must fail.

Malformed WHILE without a section target:

```text
while #i < 4
```

must fail.

Current runtime diagnostic:

```text
WHILE syntax is: while <condition> sec=<section>.
```

AI-generated WHILE must always contain both:
- a non-empty condition;
- a final section target.

---

## 13. Action registry / validator contract

Known standalone loop-control actions:

```text
break
continue
```

Known loop action:

```text
while
```

Current action contract includes the equivalent of:

```text
break;Exact;None;None;-
continue;Exact;None;None;-
while;Exact;NonEmpty;None;<condition> sec=<section>
```

The validator recognizes actions from the language action registry. Adding runtime code alone is insufficient if the action is absent from the registry.

AI rule:
when proposing a new BaselScript action, consider all three layers:

```text
language contract / validator
parser representation
runtime dispatch + implementation
```

---

## 14. Parser behavior relevant to control flow

`break` and `continue` are parameterless actions and therefore need to be accepted as standalone action names with empty content.

WHILE has content and is parsed by the generic action/content path:

```text
source:
while #i < 4 sec=loop_body

parsed:
action  = while
content = #i < 4 sec=loop_body
```

The parser does not need an ENDWHILE structure for WHILE_V1.

Nested IF is different: explicit ELSE and ENDIF runtime markers are preserved so nested branch state can be restored correctly.

---

## 15. Runtime dispatch requirement

Implementing a function is not enough.

WHILE must be routed by the runtime action dispatcher:

```text
action == while
    -> ActionWhile(...)
```

This was a real defect found during implementation: WHILE was parsed and validated correctly, but its body did not execute because the dispatcher did not route `while` to `ActionWhile`.

AI review rule:
for every action, verify the full pipeline:

```text
source text
-> parser
-> Actions table
-> validator/action registry
-> Action()
-> ActionBearbeitung / dispatcher
-> concrete runtime handler
-> return/continuation mechanics
```

---

## 16. Confirmed regression status

Current confirmed Windows reference status:

```text
NESTED_IF_RUNTIME_V1_OK
NESTED_IF_VALIDATOR_V1_OK
LOOP_CONTROL_V1_OK
WHILE_V1_OK
```

Confirmed regression areas:

- legacy IF;
- nested IF;
- ELSEIF;
- ELSE;
- false parent gating;
- section calls around nested IF;
- return from called section;
- three-level IF nesting;
- IF inside FOR;
- FOR inside IF;
- BREAK;
- CONTINUE;
- BREAK/CONTINUE inside nested IF;
- negative FOR step;
- BREAK/CONTINUE outside loop errors;
- basic WHILE;
- initially false WHILE;
- WHILE parentheses;
- nested WHILE parentheses;
- compound logical WHILE condition;
- CONTINUE in WHILE;
- BREAK in WHILE;
- IF inside WHILE;
- WHILE inside IF;
- WHILE inside WHILE;
- inner BREAK affects only inner loop;
- FOR inside WHILE;
- WHILE inside FOR;
- malformed WHILE diagnostics.

---

## 17. AI generation rules

When generating BaselScript control flow:

1. Use BaselScript syntax, not syntax borrowed from another language.
2. Close every IF with ENDIF.
3. Use section-based WHILE:
   `while <condition> sec=<section>`.
4. Do not invent ENDWHILE for WHILE_V1.
5. Ensure WHILE body changes state when needed to avoid infinite loops.
6. Use BREAK only inside an active loop.
7. Use CONTINUE only inside an active loop.
8. Assume BREAK and CONTINUE act on the nearest loop.
9. Preserve existing FOR syntax when editing existing code.
10. Prefer simple, explicit conditions over unnecessarily clever expressions.
11. For nested loops, use separate index/state variables unless intentional sharing is required.
12. When reviewing a language feature, verify parser + validator + runtime, not only one layer.

---

## 18. Recommended code patterns

### Guard-and-continue

```text
SECTION body
    #i=#i + 1

    if #skip == 1
        continue
    endif

    call sec=process_item
END
```

Use when one iteration should be skipped without ending the loop.

### Search-and-break

```text
SECTION body
    if #found == 1
        break
    endif

    call sec=check_item
END
```

Use when the first matching item is enough.

### Bounded WHILE

```text
#i=0
while #i < #count sec=body

SECTION body
    call sec=process_item
    #i=#i + 1
END
```

Use for explicit state-driven iteration.

### Nested-loop isolation

```text
#outer=0
while #outer < #outer_count sec=outer_body

SECTION outer_body
    #inner=0
    while #inner < #inner_count sec=inner_body
    #outer=#outer + 1
END

SECTION inner_body
    if #done == 1
        break
    endif

    #inner=#inner + 1
END
```

Use separate loop state and rely on nearest-loop BREAK semantics.

---

## 19. Known implementation note

The Windows runtime is the current reference behavior for these features.

Android should reproduce the language semantics, but does not have to reproduce the same C# internal data structures.

For Android porting, preserve at minimum:

```text
nested IF branch stack semantics
nearest-loop BREAK
nearest-loop CONTINUE
parent loop-context restoration
WHILE condition reevaluation
WHILE section dispatch
standalone break/continue parsing
action registry / validator entries
```
